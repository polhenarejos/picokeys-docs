# OTP fuses

Here, OTP means one-time-programmable chip fuses, not Yubico one-time passwords.

PicoKeys uses the same logical idea on RP2350/RP2354 and ESP32-S3 class devices, but the hardware names are different:

- RP2350/RP2354: OTP rows and page locks.
- ESP32-S3: eFuse key blocks, purpose fields, secure-boot enable bits, revoke bits, and debug-disable bits.

The source of truth for the current implementation is `pico-keys-sdk/src/fs/otp.c` and `pico-keys-sdk/src/fs/otp.h`.

!!! danger
    These writes are permanent. A wrong OTP/eFuse write can make a board unrecoverable or unable to boot unsigned/development firmware.

## Two different OTP meanings

Do not confuse these:

- **Chip OTP/eFuse**: irreversible hardware state used for root keys, secure boot, lockout, and sealing.
- **Pico FIDO OTP slots**: Yubico OTP, HOTP, static password, and challenge-response credentials.

This page is only about chip OTP/eFuse state.

## Logical keys

PicoKeys initializes two 32-byte hardware-backed values when the relevant OTP/eFuse storage is empty:

| Logical name | Code name | Purpose |
| --- | --- | --- |
| Key 1 | `otp_key_1` | Random 32-byte key used as the device root / MKEK masking material. |
| Key 2 | `otp_key_2` | Generated secp256k1 private key used by rescue / secure operations that need a device private key. |

The exact storage differs by platform.

## RP2350 / RP2354

For RP2350-class builds, the SDK defines:

| Logical value | OTP row | Notes |
| --- | ---: | --- |
| `otp_key_1` / MKEK | `0xE90` | New MKEK row, 32 bytes. |
| `otp_key_2` / DEVK | `0xE80` | New device-key row, 32 bytes. |
| old MKEK migration source | `0xEF0` | Read if present, migrated to `0xE90`, then invalidated. |
| old DEVK migration source | `0xED0` | Read if present, migrated to `0xE80`, then invalidated. |

After writing each new key, the firmware writes anti-imaging chaff:

| Key row | Chaff row |
| ---: | ---: |
| `0xE90` | `0xEB0` |
| `0xE80` | `0xEA0` |

The chaff is the bitwise complement of the raw key data, written at `row + 32`.

### RP2350 page lock

After writing or migrating the MKEK/DEVK material, the firmware locks the OTP page containing those rows:

| Page | Rows covered | Why |
| --- | --- | --- |
| page `58` | `0xE80` to `0xEBF` | Protects MKEK/DEVK and chaff rows from later direct access according to the page lock policy. |

The code calls `otp_lock_page(OTP_MKEK_ROW >> 6)`, and `0xE90 >> 6` is page `58`.

### RP2350 secure boot

When secure boot is enabled through `otp_enable_secure_boot()`, these RP2350 OTP areas are modified:

| Region / constant | What is burned |
| --- | --- |
| `OTP_DATA_BOOTKEY0_0_ROW + 0x10 * bootkey` | The 32-byte secure-boot public-key hash used by the boot ROM. |
| `OTP_DATA_BOOT_FLAGS1_ROW` plus redundancy rows `R1` and `R2` | Marks the selected boot key as valid. If secure lock is requested, marks every other key invalid. |
| `OTP_DATA_CRIT1_ROW` plus redundancy rows `R1` through `R7` | Sets `SECURE_BOOT_ENABLE`. If secure lock is requested, also sets `DEBUG_DISABLE`, `GLITCH_DETECTOR_ENABLE`, and maximum glitch-detector sensitivity bits. |
| `OTP_DATA_PAGE1_LOCK1_ROW` | If secure lock is requested, makes page 1 bootloader-read-only. |
| `OTP_DATA_PAGE2_LOCK1_ROW` | If secure lock is requested, makes page 2 bootloader-read-only. |

The secure-boot key hash currently used by the SDK is the fixed 32-byte value also present in `config/rp2350/secure_boot.json`.

### RP2350 anti-rollback

The current PicoKeys SDK code inspected here enables secure boot and secure lock, but this documentation should not claim a PicoKeys-specific rollback fuse map unless the provisioning tool for the target firmware exposes and documents it.

The RP2350 boot ROM has rollback support, but burning rollback state is a separate production decision from burning MKEK/DEVK and secure-boot state.

## ESP32-S3

ESP32-S3 does not use RP2350-style OTP rows. It uses eFuse key blocks and named eFuse fields.

For ESP builds, the SDK defines:

| Logical value | eFuse storage | Key purpose |
| --- | --- | --- |
| `otp_key_1` | `EFUSE_BLK_KEY3` | Written with `ESP_EFUSE_KEY_PURPOSE_USER`. |
| `otp_key_2` | `EFUSE_BLK_KEY4` | Written with `ESP_EFUSE_KEY_PURPOSE_USER`. |

After writing either key block, the firmware write-protects:

- the key block itself with `esp_efuse_set_key_dis_write()`
- the key-purpose field with `esp_efuse_set_keypurpose_dis_write()`

### ESP32-S3 secure boot

When `otp_enable_secure_boot()` is called on ESP32-S3:

| eFuse state | What is burned |
| --- | --- |
| unused key block selected by `esp_efuse_find_unused_key_block()` | Stores the fixed 32-byte secure-boot digest used by PicoKeys. |
| selected key block purpose | Set to `SECURE_BOOT_DIGEST0`, `SECURE_BOOT_DIGEST1`, or `SECURE_BOOT_DIGEST2` depending on bootkey index. |
| `ESP_EFUSE_SECURE_BOOT_EN` | Enables secure boot if not already enabled. |

If a digest for the requested index already exists, the code verifies that it matches the expected PicoKeys digest before continuing.

### ESP32-S3 secure lock

If secure lock is requested, the SDK additionally burns:

| eFuse state | Effect |
| --- | --- |
| digest revoke bits for every non-selected digest index | Revokes all secure-boot digest slots except the selected one. |
| selected secure-boot key block write-disable | Prevents rewriting the secure-boot digest block. |
| selected secure-boot key-purpose write-disable | Prevents changing the digest block purpose. |
| `ESP_EFUSE_SOFT_DIS_JTAG`, when available | Disables software JTAG. |
| `ESP_EFUSE_HARD_DIS_JTAG`, when available | Disables hardware JTAG. |
| `ESP_EFUSE_DIS_USB_JTAG`, when available | Disables USB JTAG. |
| `ESP_EFUSE_DIS_USB_SERIAL_JTAG`, when available | Disables USB serial/JTAG. |
| `ESP_EFUSE_DIS_PAD_JTAG`, when available | Disables pad JTAG. |

The code intentionally leaves ROM download mode enabled: the call to `esp_efuse_disable_rom_download_mode()` is present but commented out.

## Summary

### RP2350 / RP2354 burns

- `0xE90`: MKEK / `otp_key_1`
- `0xE80`: DEVK / `otp_key_2`
- `0xEB0`: complement chaff for `0xE90`
- `0xEA0`: complement chaff for `0xE80`
- page 58 lock for the MKEK/DEVK page
- secure-boot bootkey row for the selected boot key
- `BOOT_FLAGS1` valid bit for selected boot key
- optional `BOOT_FLAGS1` invalid bits for other boot keys when secure lock is requested
- `CRIT1.SECURE_BOOT_ENABLE`
- optional `CRIT1.DEBUG_DISABLE`, `CRIT1.GLITCH_DETECTOR_ENABLE`, and glitch sensitivity when secure lock is requested
- optional page 1 and page 2 bootloader-read-only locks when secure lock is requested

### ESP32-S3 burns

- `EFUSE_BLK_KEY3`: `otp_key_1`, purpose `USER`
- `EFUSE_BLK_KEY4`: `otp_key_2`, purpose `USER`
- write-disable for KEY3/KEY4 and their key-purpose fields
- one secure-boot digest key block, purpose `SECURE_BOOT_DIGEST0/1/2`
- `ESP_EFUSE_SECURE_BOOT_EN`
- optional revoke bits for the non-selected secure-boot digests when secure lock is requested
- optional write-disable for the selected secure-boot digest block and purpose
- optional JTAG / USB-JTAG / USB-serial-JTAG disable bits when secure lock is requested

## Conservative operating rule

Before any fuse-burning workflow:

1. confirm the exact chip family
2. confirm the exact firmware build and provisioning command
3. test on non-critical hardware
4. record the before and after state

If you cannot satisfy those four conditions, do not burn fuses.

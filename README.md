# DerpFest 17 for POCO X5 Pro 5G (redwood) — Installation Guide

This guide takes you from an unlocked phone to DerpFest 17, step by step.

> ⚠️ **Installing wipes everything on the phone.** Back up photos, chats (WhatsApp/Telegram backups),
> 2FA codes and anything else you need **before** you start.

---

## What you need

- **Phone:** POCO X5 Pro 5G / Redmi Note 12 Pro Speed (codename **redwood**), charged to at least 50%
- **Unlocked bootloader** and firmware from Global HyperOS 2 **`OS2.0.17.0.UMSMIXM`**
- **A PC** (Windows, Linux or macOS) and a USB **data** cable
- **Android platform-tools** (`adb` and `fastboot`):
  [download from Google](https://developer.android.com/tools/releases/platform-tools)
- **Windows only:** the [Google USB driver](https://developer.android.com/studio/run/win-usb), so
  the PC sees the phone in fastboot
- **The ROM files:**

  | File | What it is |
  |---|---|
  | `DerpFest-v17-20261017-redwood-Community-Stable.zip` | The ROM |
  | `boot.img` | Kernel (Vajra 4.0 with KernelSU-Next) |
  | `vendor_boot.img` | Recovery and vendor ramdisk |
  | `dtbo.img` | Device tree overlay |

Put all four files in the platform-tools folder, so the commands below work without full paths.

---

## Step 1 — Check the download (optional, recommended)

Make sure the zip downloaded completely:

```
# Linux / macOS
sha256sum DerpFest-v17-20261017-redwood-Community-Stable.zip

# Windows (PowerShell)
Get-FileHash DerpFest-v17-20261017-redwood-Community-Stable.zip
```

It must print:

```
7e1d394cf3ff0e8bcc1d31e173242cc047183a1152ffb33dc1c4278d45c04c87
```

If it doesn't match, download the zip again.

---

## Step 2 — Flash the boot images

1. Turn the phone off. Hold **Power + Volume down** until the **FASTBOOT** screen appears.
2. Connect it to the PC and check it's detected:

   ```
   fastboot devices
   ```

   You should see a serial number. If nothing shows up, try another USB port or cable, or install
   the USB driver (Windows).
3. Flash the three images:

   ```
   fastboot flash boot boot.img
   fastboot flash dtbo dtbo.img
   fastboot flash vendor_boot vendor_boot.img
   ```

   Each one must end with `OKAY`.
4. Reboot into the new recovery:

   ```
   fastboot reboot recovery
   ```

---

## Step 3 — Format data

In the recovery (use the volume keys to move, the power key to select, or tap the screen):

1. **Factory reset → Format data / factory reset**
2. Confirm. This erases all data, including internal storage.
3. Go back to the main menu.

> This step is required when you come from stock or another ROM. Without it the phone won't boot.

---

## Step 4 — Install the ROM

1. In the recovery, choose **Apply update → Apply from ADB**.
2. On the PC, run:

   ```
   adb sideload DerpFest-v17-20261017-redwood-Community-Stable.zip
   ```

3. Wait until it finishes, which takes several minutes.
   - The PC may stop at about **47%** or show `Total xfer: 1.00x`. **That's normal.**
     Check the phone's screen: it should say the install succeeded.
4. If the recovery asks to **reboot to recovery to install additional packages**, choose **No**.
5. Choose **Reboot system now**.

The first boot takes 3–5 minutes. Don't interrupt it.

---

## Step 5 — After the first boot

- **Root (optional):** the kernel already includes KernelSU-Next. Install the manager app:
  [KernelSU-Next 3.4.2](https://github.com/KernelSU-Next/KernelSU-Next/releases/download/v3.4.2/KernelSU_Next_v3.4.2_33355-release.apk).
- **Google apps:** included. Sign in to Google as usual.
- **Casting to Chromecast / Google TV:** Android 17 asks every app for permission to find devices
  on your Wi-Fi. If YouTube or another app doesn't see your TV, go to **Settings → Apps → (the
  app) → Permissions → Nearby devices → Allow**.

---

## Updating to a newer build

Updates of this ROM don't need a data format, and you keep your apps and files.

**Over the air (easiest):** go to **Settings → System → System updates**, tap **Download**, then
**Install**, and reboot when it asks. Works from the 16 October 2026 build (Vajra 4.1) onwards.

**Or by sideload:**

1. Reboot to recovery (`adb reboot recovery`, or **Power + Volume up** from off).
2. **Apply update → Apply from ADB**, then `adb sideload <new zip>`.
3. **Reboot system now**.

> Format data again only when you come from a **different ROM** or a build signed with
> different keys (such as a test build).

---

## Good to know

- **64-bit only:** the ROM runs a 64-bit only zygote to save RAM. Apps that ship only 32-bit code
  won't open. Almost every Play Store app is 64-bit.
- **Play Integrity:** the ROM passes basic checks. Strong integrity needs your own keybox, which
  the ROM doesn't include.
- **Banking or DRM apps** may notice the unlocked bootloader or root. Hide root from them in the
  KernelSU-Next manager.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `fastboot devices` shows nothing | Use another USB port or cable; on Windows install the Google USB driver |
| `adb sideload` says "no devices" | Make sure the recovery is in **Apply from ADB** mode, then retry |
| Stuck on the boot animation for more than 10 minutes | Go back to recovery and **Format data** again (Step 3) |
| Phone boots back into recovery | Format data again, then sideload the ROM again |
| Want to go back to stock | Flash the official fastboot ROM with Mi Flash, **"clean all"** (never "clean all and lock") |

---

## Credits

- [@itsraiharshit](https://t.me/itsraiharshit) and
  [Redwood-AOSP](https://github.com/Redwood-AOSP) for the device and vendor trees
- genie for the [Vajra kernel](https://github.com/genie1997/android_kernel_xiaomi_redwood)
- Hugo and StevenMX for the Adreno v863.1 Vulkan driver
- The DerpFest team

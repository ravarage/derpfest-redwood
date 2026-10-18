# Changelog

## 17 October 2026

### Source

- DerpFest 17, based on Android 17 (`CP2A.260605.016`)
- Android security patch: **1 October 2026**
- Synced with the latest sources (Redwood-AOSP device and vendor trees)
- Build type: `user`, signed with private keys

### Device

**Updates**
- OTA is on: this build installs from **Settings → System → System updates** on the 16 October build, no PC and no data wipe needed. Later builds arrive the same way

**Graphics**
- The whole 64-bit Adreno stack is now v863.1: OpenGL ES, OpenCL and the GPU tone mapper join the Vulkan driver
- Fixed browsers (Brave, Chrome) flickering black: their GPU process crashed in the new driver; that call now goes to v863's own helper
- Camera, HDR video and OpenCL apps (Geekbench, MACE in the camera) tested on the new drivers
- 32-bit graphics, gralloc and the GPU firmware stay stock

**Audio and microphone**
- Fixed empty voice notes in WhatsApp (and other apps that record: Telegram, recorders, voice search). The audio driver sent a recording to a broken capture stream whenever it thought another recording was running, and after the first "Hey Google" it thought that forever
- "Hey Google" runs on the audio DSP again (sound trigger HAL and Voice Match enrollment restored), so it no longer keeps the microphone open all the time
- Ambient Music (Now Playing) no longer holds the microphone until reboot when the input gives no sound
- ViPER4AndroidFX included (from Redwood-AOSP)

**System**
- Fixed the lock screen clock showing GMT (wrong time) after a reboot when a root module (for example BRENE) rewrote Android's saved settings with the wrong permissions: the time zone and other saved settings are no longer lost at boot

**Kernel**
- Vajra 4.1 (Linux 5.4.302) with KernelSU-Next and SuSFS, unchanged

## 16 October 2026

### Source

- DerpFest 17, based on Android 17 (`CP2A.260605.016`)
- Android security patch: **1 October 2026**
- Build type: `user`, signed with private keys (not the public test keys)
- Google apps included

### Device

**Graphics**
- Adreno Vulkan driver v863.1 (64-bit) with its six helper libraries
- Vulkan 1.3 (the stock driver reports 1.1)
- The UI (HWUI) renders with SkiaVK
- SurfaceFlinger's RenderEngine runs on SkiaVK (`skiavkthreaded`)
- 32-bit Vulkan and all OpenGL ES libraries stay stock

**Memory**
- zram compresses with zstd, about 4.25:1 on device (lz4 gets about 2:1)
- Fixed the zram compressor: the old `lz4kd` setting never worked, so zram stayed on lz4
- 64-bit only zygote: frees RAM, boots a bit faster. Apps that ship only 32-bit code won't run

**Network**
- BBR TCP congestion control, for steadier mobile data and Wi-Fi

**Kernel**
- Vajra 4.1 (Linux 5.4.302) with KernelSU-Next and SuSFS
  - fewer CPU wakeups while idle, light tasks prefer the little cores
  - modem and DSP wakeup reasons recorded for battery stats
- zstd is the kernel's default zram compressor

**Updates**
- OTA updates: future builds arrive in **Settings → System → System updates**, no PC needed

**Apps**
- VibePlayer music player included

Full technical details of the device changes: the commit history of
[ravarage/android_device_xiaomi_redwood](https://github.com/ravarage/android_device_xiaomi_redwood/commits/seventeen)
and [ravarage/android_vendor_xiaomi_redwood](https://github.com/ravarage/android_vendor_xiaomi_redwood/commits/seventeen)

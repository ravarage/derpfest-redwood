# Changelog

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

Full technical details of the device changes:
[Redwood-AOSP/android_device_xiaomi_redwood#1](https://github.com/Redwood-AOSP/android_device_xiaomi_redwood/pull/1)

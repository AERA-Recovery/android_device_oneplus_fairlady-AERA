# Fairlady GPU recovery prebuilts

These proprietary ARM64 Adreno userspace libraries, kernel module, and Gen8
firmware are shared from the matching SM8850/Adreno 840 platform stack. They
stay in the Fairlady device tree because they are
specific to the SM8850/`canoe` platform and its Adreno 840.

`msm_kgsl.ko` is taken from the matching SM8850 vendor_dlkm image. All KGSL hard
and soft dependencies are loaded by the stock recovery module set.
`BoardConfig.mk` requests KGSL through `AERA_LOAD_VENDOR_MODULES`, using the same
dependency-aware loader as the rest of Fairlady's vendor modules.

The userspace closure includes only EGL/GLES2 and the mapper/gralloc libraries
needed by Qualcomm's Android EGL subdriver. Vulkan, SurfaceFlinger, and the rest
of the Android graphics services are intentionally excluded. The software
renderer remains the fail-safe path.

The matching native-window ABI is packaged unmodified because both the Adreno driver
and the HexLP service use it directly. No proprietary binary is patched.

The validation probe creates an EGL pbuffer, renders a known pixel, and reads it
back. It provides an end-to-end check of KGSL, gen80200 firmware, EGL/GLES2, and
the Adreno 840 shader compiler before hardware rendering is trusted.

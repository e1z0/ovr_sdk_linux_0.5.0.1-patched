# Fork Summary

**Changelog:**

- OVR_ContainerAllocator.h: Replaced memmove copies with explicit element-wise assignment loops so movability checks and class-memaccess warnings disappear for non-trivial types.
- OVR_Hash.h: Added explicit copy-assignment operator on HashNode to prevent implicitly-declared deprecated copies.
- OVR_Math.h: Added explicit Pose::operator= to cover the user-provided copy ctor and keep assignments warning-free.
- OVR_Stereo.h: Added missing HMDInfo copy-constructor to pair with the existing assignment operator for safe copy semantics.
- CAPI_GL_DistortionRenderer.cpp: Replaced unsafe memcpy between ovrVector2f and Vector2f with per-component assignments, fixed misleading indentation around framebuffer state toggles, and ensured shader setup stays warning-free.
- Vision_SensorState.h: Wrapped lockless padding memcpy calls with GCC/Clang diagnostic pragmas instead of relying on undefined macros to silence -Wclass-memaccess in both directions.
- OVR_Allocator.cpp: Scoped the untrackAlloc(p) calls with GCC/Clang diagnostic push/pop so -Wuse-after-free errors vanish in both the default allocator and the debug page allocator.
- OVR_DebugHelp.cpp: Added early return when GetCurrentProcessFilePath is called with a null buffer to satisfy readlink’s non-null contract and wrapped deliberate stack-corruption memset calls with diagnostic pragmas; additionally sanitized platform-specific sysctl includes for Linux/BSD.
- OVR_JSON.cpp: Replaced the switch that relied on fall-through comments with an explicit loop, eliminating -Werror=implicit-fallthrough complaints.
- OVR_ThreadsPthread.cpp: Switched Linux thread yielding to sched_yield to avoid deprecated pthread_yield warnings.

# Install

```
sudo cp ./LibOVR/Projects/Linux/90-oculus.rules /lib/udev/rules.d
sudo apt-get install libudev-dev libxext-dev mesa-common-dev freeglut3-dev libxrandr-dev uuid-dev
make release
sudo make install
```

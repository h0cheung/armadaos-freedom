# QSEED rotation calculator

The shared kernel enables the QSEED compatibility adapter by default on ARM64
when its Kconfig dependencies are met. It uses `qcom/libqseed3.so`, version
1.5.7 (SHA-256 `edf56556ef878c24cfc16d04a57e0cea50bfba2015a1aca85d510f870ecbac39`),
extracted from `vendor/lib64/libqseed3.so` in a TB322FC stock Android image.
The library is a proprietary Android ARM64 ELF. Its redistribution terms have
not been identified in the available image materials.

The adapter executes the pinned binary inside the kernel, with fixed ABI and
relocation metadata, RX code and RW/NX data. It is not a general ELF loader.
It currently handles QSEED 3.4, packed 32-bit RGB, integral source rectangles,
DRM rotate-270 / hardware rotate-90 and native or upscaled geometry. The plane
must begin at the output origin. Other combinations and failed calculations use
the existing scaler. Other QSEED hardware versions are not covered by this
library integration. Set `msm.dpu_binary_qseed=N` to compare the existing scaler;
changes take effect on the next plane update.

The original TB322FC implementation passed ARM64 QEMU comparisons against the
stock library for 18 geometries and was observed in device register captures.
This generalized adapter has additional offline checks for five output sizes;
visual results and behavior on other devices remain to be tested.

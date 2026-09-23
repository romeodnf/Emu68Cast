# Emu68Cast

A bare-metal HDMI casting service for Pistorm, built on Emu68.

The service runs on CPU core 3 in reserved memory and is loaded separately as `Emu68Cast.bin`. The Emu68 loader validates the image, reserves its memory and starts the service. The design aims to keep changes to the Emu68 core as small as possible, with capture, encoding and networking implemented in the separate service. Source includes display capture, experimental H.264 encoding and Wi-Fi/IPv4/UDP support, plus Amiga diagnostics and preferences tools.

Emu68Cast captures RTG output and, when Framethrower is installed, native Amiga video.

Emu68Cast is designed as a separate, optional service rather than a feature incorporated into Emu68 or Framethrower. Its integration is intended to preserve the normal operation of both projects.

Hardware testing has verified bounded RTG encoding. Continuous network streaming remains experimental and is not yet hardware validated.

Initially designed for use with Kodi, with support for additional services planned for future versions.

Tested on PiStorm Classic with a Raspberry Pi 3B+ and Framethrower. Raspberry Pi 4 and Compute Module 4 (CM4) are expected to work but have not yet been verified. Ethernet connectivity is planned for a future version.

## Source

- `emu68cast/service/`: service entry point and display capture.
- `src/emu68cast/`: loader and encoder integration.
- `emu68cast/experimental/`: networking and stream components.
- `emu68cast/amiga/`: diagnostics and preferences.
- `emu68cast/shared/`, `include/emu68cast.h`: settings and shared interface.
- `emu68cast/tests/`, `emu68cast/tools/`: tests and build/support tools.

## Build and load

Initialize the submodules with `git submodule update --init --recursive`. Build with CMake and the supplied AArch64 cross-toolchain, selecting `TARGET=raspi64`, `VARIANT=pistorm-classic` and `EMU68CAST_BOOTSTRAP=ON`. The service builds alongside the kernel. Experimental encoder and radio modes require their matching build options and device assets.

Load `Emu68Cast.bin` after the Kickstart ROM in `initramfs` and enable the `emu68cast` overlay. See `documentation/overlays.md` for memory options. Build Amiga tools separately using `emu68cast/amiga/Makefile` and an Amiga cross-compiler.

Upstream source and license notices are retained. See `LICENSE`.

## Wi-Fi usage

Emu68Cast takes exclusive control of the Raspberry Pi’s onboard Wi-Fi while the service is running. The Wi-Fi interface is dedicated to Emu68Cast and cannot be used simultaneously by AmigaOS.

## Settings and passwords

The Amiga preferences tool, `Emu68CastPrefs`, saves Wi-Fi credentials, the Kodi address and control port, and optional Kodi login details to `EMU68BOOT:Emu68Cast.cfg`. Settings stay separate from the service executable and can be changed without rebuilding it.

Load the settings at startup by adding `Emu68Cast.cfg` immediately after `Emu68Cast.bin` on the existing `initramfs` line. A matching loader/service and a full Pi restart are required to apply boot settings.

Passwords are visible in the editor and stored unencrypted on the SD card. At startup, the loader validates the settings, copies them into private reserved memory outside the Amiga diagnostic mapping, and clears the original settings trailer. Credentials are excluded from diagnostic output, boot logs and device-tree properties. This reduces accidental exposure; it does not protect against SD-card or privileged-memory access. Encryption of stored credentials is planned for a future version.

## Acknowledgments

Thank you to Michal Schulz, author of [Emu68](https://github.com/michalsc/Emu68), and Claude Schwarz, author of [Framethrower](https://github.com/PiStorm/Framethrower_Denise), along with the contributors to both projects.

The source builds on Emu68, retaining its existing code, bundled components and license notices. This includes the embedded `unicam.resource` used for native-video capture.

Other repositories consulted during development:

- [WiFiPi.device](https://github.com/michalsc/WiFiPi.device): Wi-Fi initialization, SDIO access and GPIO routing.
- [unicam.resource](https://github.com/michalsc/unicam.resource): native-video capture initialization and display configuration.
- [Emu68-tools-old](https://github.com/michalsc/Emu68-tools-old): SD-card driver initialization and storage-controller routing.
- [Framethrower](https://github.com/PiStorm/Framethrower_Denise): video format, CSI-2 transport and compatibility.
- [Circle](https://github.com/rsta2/circle) and [Linux](https://github.com/torvalds/linux): SDIO, wireless initialization and protocol references.

The development notes record these as references, with no new third-party driver source copied into the Emu68Cast networking modules. Framethrower source and firmware are unchanged.

## About this project

Emu68Cast is a personal hobby project, developed for enjoyment and experimentation with AI-assisted software development using ChatGPT 6 Astra and Codex. The service code was generated with AI, with testing and direction provided by me.

No AIs were harmed in the making of this service.

Amiga rules!

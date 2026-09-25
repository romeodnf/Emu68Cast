# Emu68Cast

Emu68Cast is a video-streaming service for PiStorm, built on Emu68. It captures RTG video or native Amiga video through Framethrower and streams H.264 over the Raspberry Pi's Wi-Fi to Kodi or VLC.

The service runs on core 3 and stays available between streaming sessions. Amiga tools provide MUI preferences, start/stop control, diagnostics and logging.

Wi-Fi runs on the PiStorm side and is dedicated to Emu68Cast. The Wi-Fi interface is unavailable to AmigaOS while the service is running.

## Hardware

The target is Raspberry Pi 3. Framethrower is required for native Amiga video capture.

## Service status — 25 September 2026

Wi-Fi streaming, separate Kodi/VLC profiles, preferences, startup options and session controls are implemented. Kodi supports optional automatic playback control.

Automatic resolution changes and switching between RTG and Framethrower during streaming are implemented and need testing.

The experimental service bridge is intended to provide two-way communication between the Emu68Cast service and AmigaOS, with ARexx support for control, automation and testing. It still needs verification and testing.

Automatic recovery when the video source becomes temporarily unavailable is implemented, but needs testing.

Development and hardware testing are ongoing. Sustained frame rates, latency and receiver playback still need further measurement.

## Planned work

- Ethernet support alongside Wi-Fi.

- Automatic frame-rate selection where feasible: RTG 60/30 fps and PAL Framethrower 50/25 fps, based on measured performance.
- Further performance testing and improvements with Kodi and VLC.

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

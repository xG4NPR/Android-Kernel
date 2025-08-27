<img width="1886" height="1040" alt="Image" src="https://github.com/user-attachments/assets/b4bc1219-a5f1-4e7d-98f3-f91560b5bf7c" />


Shorekeeper Kernel(Unstable)

Upstream: 6.6.42

Version:0.2

Android Version: 16

Device:Redmi Note 10S(Secret)

Build Type: Experimental (Custom DTBO-based)
Date: 27 August 2025

Changes and Bug Fixes:
Upstreamed Linux kernel to 6.6.42 for improved scheduling, memory handling and security hardening.

Custom DTBO overlay integration

Panel timing overrides added to address early blackouts and display sync issues

Integrated MediaTek platform updates from the 2025 MTK patch series:

mtk_disp_drv — r48p2

mtk_imgsys_drv — r36p1

mtk_mdp_core — r51p0

mtk_cam_dev — r27p3

mtk_vcodec — r32p2 (addresses HEVC 10-bit decode stability)

mtk_drm_panel — r18p5

Updated CPU freq and GPU freq scaling tables to reflect Helio G95 operating points.

Thermal trip points adjusted via thermal-zones node to improve GPU throttling behavior.

mali,gpu-mem-clk DTS node tuned for stable GPU DVFS in the 650–900 MHz range.

Updated mtk_cam_dev to r27p3.

Improved ISP memory alignment and buffer handling in mtk_imgsys_drv to reduce underruns

Improved AudioFlinger buffer handling and low-latency path selection for multi-stream playback scenarios.

Patched VoLTE stability logic in mtk_ims to improve behavior on multi-SIM configurations.

Scheduler tuning optimized to improve thermal-to-power balance during mixed workloads.

Integrated updated thermal drivers and mappings:

mtk_thermal_core — r14p1

mtk_ts_pa — r12p3

mtk_ts_battery — r9p8

Raised GPU critical throttle trip point to 84 °C (previously 78 °C) to avoid overly aggressive throttling under short sustained loads.

Corrected inconsistencies in CPU freq tables; Cortex-A76 big cluster now reports full max at 2.05 GHz on validated silicon.

Kernelsu-Next Included

Known Issues:

Black screen after 6 minutes of usage 
(Fixed)

Root cause: panel idle timeout and LPM entry conflict between mtk_drm_panel and the new DTBO overlays which allowed the panel to prematurely enter a deep idle/power-off sequence while the display client (SurfaceFlinger / video decoder) still expected an active display clock.

Applied fix: overridden the panel LPM/idle behavior and prevented premature power gating at panel level by adding a DTBO override and a conservative kernel-side safeguard that forces a refresh/keepalive before the panel idle window closes.

LTE fallback during certain handover conditions(Fixed)
by improving RRC handover handling.

intermittent autofocus stall when switching between 0.6× → 1× 
(Fixed)
by improving sensor switch sequencing and stabilizing the AF state machine.

High idle drain (~5–7% overnight) caused by persistent
wakelocks originating from vendor logging/diagnostic services.

1080 @ 60 fps EIS is not available — EIS remains disabled until the MediaTek EIS HAL is fully synced with the 6.6 kernel/DTBO changes (workaround: 1080p@30fps for stabilized EIS).

Broken due to limitation(maybe):

HDR video capture may fail intermittently in low-light conditions while vendor ISP tuning is finalized.

4K @ 30 fps recording functions, freezes

High idle drain persists despite DTBO thermal tuning and scheduler improvements.

VoWiFi call connection delay (~2–3 s) remains on certain carriers.

IMS registration instabilities observed after toggling airplane mode.

Random in-call audio drops when switching between VoLTE Wi-Fi Calling remain under investigation.

High-impedance wired headset auto-detection is inconsistent

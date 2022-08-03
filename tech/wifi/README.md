# Wifi

So the wifi maintainer has stepped down: [Linux's Sole Wireless/WiFi Driver Maintainer Is Stepping Down](https://www.phoronix.com/news/Linux-Wireless-Maintainer-2025)

# Piece of shit Qualcomm Atheros QCNFA765

Using the `ath11k` driver and firmware.

Broken on Lenovo: [QCNFA765 Linux ath11k wifi crippled, high latency, packet loss, frequent disassociations? T14s AMD](https://forums.lenovo.com/t5/Other-Linux-Discussions/QCNFA765-Linux-ath11k-wifi-crippled-high-latency-packet-loss-frequent-disassociations-T14s-AMD/m-p/5252399)

These are the firmware versions currently used in different distros:

- [Debian bookworm](ath11k/debian_bookworm.txt)
  - WLAN.HSP.1.1-03125-QCAHSPSWPL_V1_V2_SILICONZ_LITE-3.6510.9 (2022-04-18 20:23)
- [Debian trixie](ath11k/debian_trixie.txt)
  - WLAN.HSP.1.1-03125-QCAHSPSWPL_V1_V2_SILICONZ_LITE-3.6510.41 (2024-04-17 08:34)
- [Pop!_OS 22.04](ath11k/popos_2204.txt)
  - WLAN.HSP.1.1-03125-QCAHSPSWPL_V1_V2_SILICONZ_LITE-3.6510.37 (2024-01-12 11:30)
- [Pop!_OS 24.04 (alpha 6)](ath11k/popos_2404.txt)
  - WLAN.HSP.1.1-03125-QCAHSPSWPL_V1_V2_SILICONZ_LITE-3.6510.37 (2024-01-12 11:30)
- [Ubuntu LTS 24.04.2](ath11k/ubuntu_2404_2.txt)
  - WLAN.HSP.1.1-03125-QCAHSPSWPL_V1_V2_SILICONZ_LITE-3.6510.37 (2024-01-12 11:30)

The worst experience has been with firmware `WLAN.HSP.1.1-03125-QCAHSPSWPL_V1_V2_SILICONZ_LITE-3.6510.37`. Constant dropouts and slowdowns. Sometimes complete crashes where the Wifi adapter stops responding and the OS even has a hard time rebooting.

Best experience has been with firmware `WLAN.HSP.1.1-03125-QCAHSPSWPL_V1_V2_SILICONZ_LITE-3.6510.41`. Good performance and no slowdowns. The adapter crashed completely once, so far.

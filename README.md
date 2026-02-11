# Meshtastic SX126X 3dBm Build

Daily automated builds of Meshtastic firmware with SX126X power limited to 3dBm for RA-01SH-P/RA-01SCH-P modules.

## Why 3dBm?

RA-01SH-P and RA-01SCH-P modules have integrated Power Amplifiers (PA) that require power limiting to prevent damage.

## Download

- [Latest Release](../../releases/latest)
- [Build Artifacts](../../actions)

## Target Hardware

- nRF52840 Pro Micro
- TCXO variant
- RA-01SH-P (SX1262) or RA-01SCH-P (LLCC68+) module

## Build Schedule

Automatically builds daily at 00:00 Taiwan time (16:00 UTC).

## Manual Build

```bash
git clone https://github.com/meshtastic/firmware.git
cd firmware
git submodule update --init

# Apply power limit
sed -i 's/\(RDEF([^,]*, [^,]*, [^,]*, [^,]*, [^,]*,\) [0-9]\+\(,\)/\1 3\2/g' src/mesh/RadioInterface.cpp
sed -i '/SX126X_MAX_POWER/a\
// Limit SX126X power to 3dBm\
#define SX126X_MAX_POWER 3
' variants/nrf52840/diy/nrf52_promicro_diy_tcxo/variant.h

# Build
pio run -e nrf52_promicro_diy_tcxo -j 4
```

## References

- [Meshtastic Firmware](https://github.com/meshtastic/firmware)
- [RA-01SH-P Datasheet](https://web.archive.org/web/20250828015915/https://aithinker-static.oss-cn-shenzhen.aliyuncs.com/docs/Specification/Ra-01SH-P_V1.0.3_Specification_cn_20250827.pdf)

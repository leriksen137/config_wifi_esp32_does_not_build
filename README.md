# CONFIG_WIFI_ESP32 does not build

This project has the same topology as the [zephyr example application](https://github.com/zephyrproject-rtos/example-application).

## Reproduction steps

Code for reproduction found here: https://github.com/leriksen137/config_wifi_esp32_does_not_build.git

Follow [Zephyr getting started](https://docs.zephyrproject.org/latest/develop/getting_started/index.html).

```bash
git clone https://github.com/leriksen137/config_wifi_esp32_does_not_build.git
cd config_wifi_esp32_does_not_build
west init -l --mf west.yml manifest
west update
west blobs fetch hal_espressif
west build -b esp32s2_lolin_mini app
```
See [ESP32-S2 Lolin Mini](https://docs.zephyrproject.org/latest/boards/wemos/esp32s2_lolin_mini/doc/index.html) why west `blobs fetch hal_espressif` is run.

## Console output
```bash
west build -b esp32s2_lolin_mini app --pristine
-- west build: making build dir path\to\repo\config_wifi_esp32_does_not_build\build pristine
-- west build: generating a build system
Loading Zephyr default modules (Zephyr base).
-- Application: path/to/repo/config_wifi_esp32_does_not_build/app
-- CMake version: 3.27.9
-- Found Python3: C:/Python311/python.exe (found suitable version "3.11.9", minimum required is "3.8") found components: Interpreter
-- Cache files will be written to: path/to/repo/config_wifi_esp32_does_not_build/zephyr/.cache
-- Zephyr version: 3.7.0-rc1 (path/to/repo/config_wifi_esp32_does_not_build/zephyr)
-- Found west (found suitable version "1.2.0", minimum required is "0.14.0")
-- Board: esp32s2_lolin_mini, qualifiers: esp32s2
-- Found host-tools: zephyr 0.16.1 (~/zephyr-sdk-0.16.1)
-- Found toolchain: zephyr 0.16.1 (~/zephyr-sdk-0.16.1)
-- Found Dtc: C:/ProgramData/chocolatey/bin/dtc.exe (found suitable version "1.5.0", minimum required is "1.4.6")
-- Found BOARD.dts: path/to/repo/config_wifi_esp32_does_not_build/zephyr/boards/wemos/esp32s2_lolin_mini/esp32s2_lolin_mini.dts
-- Found devicetree overlay: path/to/repo/config_wifi_esp32_does_not_build/app/boards/esp32s2_lolin_mini.overlay
-- Generated zephyr.dts: path/to/repo/config_wifi_esp32_does_not_build/build/zephyr/zephyr.dts
-- Generated devicetree_generated.h: path/to/repo/config_wifi_esp32_does_not_build/build/zephyr/include/generated/zephyr/devicetree_generated.h
-- Including generated dts.cmake file: path/to/repo/config_wifi_esp32_does_not_build/build/zephyr/dts.cmake

warning: MBEDTLS (defined at path/to/repo/config_wifi_esp32_does_not_build/zephyr/soc/nxp/imxrt\imxrt5xx\Kconfig.defconfig:24, path/to/repo/config_wifi_esp32_does_not_build/zephyr/soc/nxp/imxrt\imxrt6xx\Kconfig.defconfig:81, path/to/repo/config_wifi_esp32_does_not_build/zephyr/soc/nxp/imxrt/Kconfig.defconfig:99, modules\mbedtls\Kconfig:18) has direct dependencies (ENTROPY_GENERATOR && SOC_MIMXRT595S_CM33 && SOC_FAMILY_NXP_IMXRT) || (ENTROPY_GENERATOR && SOC_MIMXRT685S_CM33 && SOC_FAMILY_NXP_IMXRT) || (ENTROPY_GENERATOR && (SOC_SERIES_IMXRT10XX || SOC_SERIES_IMXRT11XX) && SOC_FAMILY_NXP_IMXRT) || 0 with value n, but is currently being y-selected by the following symbols:
 - WIFI_ESP32 (defined at drivers/wifi/esp32/Kconfig.esp32:3), with value y, direct dependencies DT_HAS_ESPRESSIF_ESP32_WIFI_ENABLED && !SMP && WIFI (value: y), and select condition DT_HAS_ESPRESSIF_ESP32_WIFI_ENABLED && !SMP && WIFI (value: y)Parsing path/to/repo/config_wifi_esp32_does_not_build/zephyr/Kconfig
Loaded configuration 'path/to/repo/config_wifi_esp32_does_not_build/zephyr/boards/wemos/esp32s2_lolin_mini/esp32s2_lolin_mini_defconfig'
Merged configuration 'path/to/repo/config_wifi_esp32_does_not_build/app/prj.conf'


error: Aborting due to Kconfig warnings

CMake Error at path/to/repo/config_wifi_esp32_does_not_build/zephyr/cmake/modules/kconfig.cmake:389 (message):
  command failed with return code: 1
Call Stack (most recent call first):
  path/to/repo/config_wifi_esp32_does_not_build/zephyr/cmake/modules/zephyr_default.cmake:132 (include)
  path/to/repo/smart-pump-iot-adapter/zephyr/share/zephyr-package/cmake/ZephyrConfig.cmake:66 (include)
  path/to/repo/smart-pump-iot-adapter/zephyr/share/zephyr-package/cmake/ZephyrConfig.cmake:92 (include_boilerplate)
  CMakeLists.txt:5 (find_package)


-- Configuring incomplete, errors occurred!
```

## Bug

Zephyr application does not build. Error messages unexpectedly point to nxp folders.

## Output file details

Note that the output file in "path/to/repo/config_wifi_esp32_does_not_build/build/zephyr/zephyr.dts" contains:
```
	wifi: wifi {
		compatible = "espressif,esp32-wifi";
		status = "okay";
	};
```
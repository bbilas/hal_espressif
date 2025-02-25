# SPDX-License-Identifier: Apache-2.0

if(CONFIG_SOC_SERIES_ESP32 OR CONFIG_SOC_SERIES_ESP32_NET)

  zephyr_compile_options(-Wno-unused-variable -Wno-maybe-uninitialized)

  zephyr_include_directories(
    include
    include/bt
    include/crypto
    ../esp_shared/include
    ../esp_shared/include/wifi
    ../esp_shared/components/include
    ../../components/hal/esp32/include
    ../../components/hal/include
    ../../components/hal/platform_port/include
    ../../components/esp_hw_support/include/soc
    ../../components/esp_hw_support/port/esp32
    ../../components/esp_hw_support/port/esp32/private_include
    ../../components/esp_hw_support/include
    ../../components/esp_common/include
    ../../components/esp_pm/include
    ../../components/esp_rom/include
    ../../components/esp_rom/include/esp32/rom
    ../../components/esp_rom/esp32/ld
    ../../components/esp32/include
    ../../components/soc/esp32/include
    ../../components/xtensa/include
    ../../components/xtensa/esp32/include
    ../../components/soc/include/soc
    ../../components/soc/include
    ../../components/soc/src/esp32/include
    ../../components/driver/include
    ../../components/driver/esp32/include
    ../../components/esp_phy/include
    ../../components/esp_phy/include/esp32
    ../../components/esp_phy/esp32/include
    ../../components/esp_wifi/include
    ../../components/efuse/include
    ../../components/efuse/private_include
    ../../components/efuse/esp32/include
    ../../components/efuse/esp32/private_include
    ../../components/esp_system/include
    ../../components/esp_system/port/include
    ../../components/esp_system/port/public_compat
    ../../components/esp_wifi/esp32/include
    ../../components/esp_timer/include
    ../../components/esp_timer/private_include
    ../../components/esp_netif/include
    ../../components/esp_event/include
    ../../components/log/include
    ../../components/wpa_supplicant/esp_supplicant/src
    ../../components/wpa_supplicant/esp_supplicant/include
    ../../components/wpa_supplicant/include
    ../../components/wpa_supplicant/include/esp_supplicant
    ../../components/wpa_supplicant/port/include
    ../../components/wpa_supplicant/src
    ../../components/wpa_supplicant/src/crypto
    ../../components/wpa_supplicant/src/utils
    ../../components/wpa_supplicant/src/eap_peer
    ../../components/spi_flash/include
    ../../components/spi_flash/include/spi_flash
    ../../components/spi_flash/private_include
    ../../components/bootloader_support/include
    ../../components/bootloader_support/include_bootloader
    ../../components/heap/include
    ../../components/mbedtls/port/include
    )

  zephyr_link_libraries(
    gcc
      -T${CMAKE_CURRENT_SOURCE_DIR}/src/linker/esp32.rom.alias.ld
      -T${CMAKE_CURRENT_SOURCE_DIR}/../../components/esp_rom/esp32/ld/esp32.rom.ld
      -T${CMAKE_CURRENT_SOURCE_DIR}/../../components/esp_rom/esp32/ld/esp32.rom.api.ld
      -T${CMAKE_CURRENT_SOURCE_DIR}/../../components/esp_rom/esp32/ld/esp32.rom.libgcc.ld
      -T${CMAKE_CURRENT_SOURCE_DIR}/../../components/esp_rom/esp32/ld/esp32.rom.newlib-data.ld
      -T${CMAKE_CURRENT_SOURCE_DIR}/../../components/esp_rom/esp32/ld/esp32.rom.newlib-funcs.ld
      -T${CMAKE_CURRENT_SOURCE_DIR}/../../components/soc/esp32/ld/esp32.peripherals.ld
    )

  zephyr_compile_definitions(ESP_PLATFORM)

  zephyr_library_sources(
    src/hal/windowspill_asm.S
    )

  zephyr_link_libraries_ifdef(
    CONFIG_HEAP_MEM_POOL_SIZE
    gcc
    "-Wl,--wrap=k_calloc"
    "-Wl,--wrap=k_malloc"
    )

  zephyr_sources_ifdef(
    CONFIG_SOC_FLASH_ESP32
    ../esp_shared/src/host_flash/cache_utils.c
    ../../components/spi_flash/flash_mmap.c
    ../../components/spi_flash/esp32/spi_flash_rom_patch.c
    ../../components/spi_flash/flash_ops.c
    ../../components/spi_flash/esp32/flash_ops_esp32.c
    )

  zephyr_compile_definitions(
    CONFIG_SPI_FLASH_ROM_DRIVER_PATCH
    CONFIG_SPI_FLASH_USE_LEGACY_IMPL
    )

  zephyr_library_sources_ifdef(CONFIG_COUNTER_TMR_ESP32     ../../components/hal/timer_hal.c)

  zephyr_sources_ifdef(
    CONFIG_ESP32_SPIM
    ../../components/hal/spi_hal.c
    ../../components/hal/spi_hal_iram.c
    ../../components/soc/lldesc.c
    ../../components/soc/esp32/spi_periph.c
    )

  zephyr_sources_ifdef(
    CONFIG_I2C_ESP32
    ../../components/hal/i2c_hal_iram.c
    ../../components/hal/i2c_hal.c
    )

  zephyr_sources_ifdef(
    CONFIG_I2S_ESP32
    ../../components/hal/i2s_hal.c
    )

  zephyr_sources_ifdef(
    CONFIG_ESP_SPIRAM
    ../esp_shared/src/host_flash/cache_utils.c
    ../../components/spi_flash/flash_ops.c
    ../../components/esp_hw_support/port/esp32/spiram.c
    ../../components/esp_hw_support/port/esp32/spiram_psram.c
    ../../components/esp_hw_support/port/esp32/cache_sram_mmu.c
    ../../components/bootloader_support/src/bootloader_common.c
    )

  zephyr_sources_ifdef(
    CONFIG_UART_ESP32
    ../../components/hal/uart_hal.c
    ../../components/hal/uart_hal_iram.c
    )

  zephyr_sources_ifdef(
    CONFIG_PWM_LED_ESP32
    ../../components/soc/esp32/ledc_periph.c
    ../../components/hal/ledc_hal_iram.c
    ../../components/hal/ledc_hal.c
    )

  zephyr_sources_ifdef(
    CONFIG_MCPWM_ESP32
    ../../components/hal/mcpwm_hal.c
    )

  zephyr_sources_ifdef(
    CONFIG_PCNT_ESP32
    ../../components/hal/pcnt_hal.c
    )

  zephyr_sources_ifdef(
    CONFIG_DAC_ESP32
    ../../components/hal/dac_hal.c
    ../../components/soc/esp32/dac_periph.c
    ../esp_shared/components/driver/dac_common.c
    ../esp_shared/components/driver/rtc_io_hal.c
    ../esp_shared/components/driver/rtc_io.c
    )

  zephyr_sources_ifdef(
    CONFIG_INPUT_ESP32_TOUCH_SENSOR
    ../esp_shared/components/driver/rtc_io_hal.c
    ../esp_shared/components/driver/rtc_io.c
    ../../components/soc/esp32/touch_sensor_periph.c
    ../../components/hal/esp32/touch_sensor_hal.c
    ../../components/hal/touch_sensor_hal.c
    )

  # FIXME: depending on Zephyr Kconfig options here is wrong
  if(CONFIG_PM OR CONFIG_POWEROFF)
    zephyr_sources(
      ../esp_shared/components/driver/gpio.c
      ../esp_shared/components/driver/rtc_io.c
      ../esp_shared/components/esp_hw_support/sleep_modes.c
      ../esp_shared/components/esp_pm/pm_impl.c
      ../../components/esp_hw_support/port/esp32/rtc_sleep.c
      ../../components/hal/rtc_io_hal.c
    )
  endif()

  zephyr_sources_ifdef(
    CONFIG_ADC_ESP32
    ../../components/hal/adc_hal.c
    ../esp_shared/components/driver/adc_common.c
    ../esp_shared/components/driver/rtc_io_hal.c
    ../esp_shared/components/driver/rtc_io.c
    src/esp_adc_cal/esp_adc_cal.c
    )

  if (NOT CONFIG_SOC_SERIES_ESP32_NET)
    zephyr_sources(
      ../../components/esp_system/port/soc/esp32/clk.c
    )

  endif()

  if (CONFIG_MCUBOOT)
    zephyr_include_directories(
      ../../components/esp_rom/esp32
      ../esp_shared/include/boot
      )

    zephyr_sources(
      ../../components/bootloader_support/src/esp32/bootloader_soc.c
      ../../components/bootloader_support/src/bootloader_efuse_esp32.c
      ../../components/bootloader_support/src/bootloader_flash.c
      ../../components/bootloader_support/src/bootloader_flash_config_esp32.c
      ../../components/bootloader_support/src/bootloader_mem.c
      ../../components/bootloader_support/src/bootloader_clock_init.c
      ../../components/bootloader_support/src/bootloader_console.c
      ../../components/bootloader_support/src/bootloader_random.c
      ../../components/bootloader_support/src/bootloader_random_esp32.c
      ../../components/esp_hw_support/cpu_util.c
      ../../components/esp_rom/patches/esp_rom_uart.c
      ../../components/esp_rom/patches/esp_rom_sys.c
      ../../components/hal/mpu_hal.c
      ../esp_shared/src/boot/esp_loader.c
      ../esp_shared/src/boot/bootloader_wdt.c
      ../esp_shared/src/boot/bootloader_init_common.c
      src/boot/bootloader_init.c
      )
  endif()

  zephyr_sources(
    ../../components/soc/esp32/adc_periph.c
    ../../components/soc/esp32/i2s_periph.c
    ../../components/soc/esp32/gpio_periph.c
    ../../components/soc/esp32/rtc_io_periph.c
    ../../components/esp_hw_support/esp_clk.c
    ../../components/esp_hw_support/port/esp32/rtc_clk.c
    ../../components/esp_hw_support/port/esp32/rtc_init.c
    ../../components/esp_hw_support/port/esp32/rtc_time.c
    ../../components/esp_hw_support/port/esp32/rtc_clk_init.c
    ../../components/esp_hw_support/mac_addr.c
    ../../components/esp_hw_support/regi2c_ctrl.c
    ../../components/esp_timer/src/ets_timer_legacy.c
    ../../components/esp_timer/src/esp_timer.c
    ../../components/esp_timer/src/esp_timer_impl_lac.c
    ../../components/driver/periph_ctrl.c
    ../../components/log/log_noos.c
    ../../components/log/log.c
    ../../components/hal/wdt_hal_iram.c
    ../../components/hal/interrupt_controller_hal.c
    ../../components/hal/esp32/interrupt_descriptor_table.c
    ../../components/efuse/esp32/esp_efuse_fields.c
    ../../components/efuse/esp32/esp_efuse_table.c
    ../../components/efuse/esp32/esp_efuse_utility.c
    ../../components/efuse/src/esp_efuse_api.c
    ../../components/efuse/src/esp_efuse_api_key_esp32.c
    ../../components/efuse/src/esp_efuse_utility.c
    ../../components/bootloader_support/src/bootloader_flash_config_esp32.c
    ../../components/bootloader_support/src/bootloader_efuse_esp32.c
    ../../components/esp_system/port/soc/esp32/reset_reason.c
    ../esp_shared/src/common/esp_system_api.c
    ../esp_shared/src/common/heap_caps.c
    src/common/dport_access.c
    src/stubs.c
    )

  ## shared WIFI/BT resources
  if (CONFIG_BT OR CONFIG_WIFI_ESP32)
    zephyr_sources(
      ../../components/esp_phy/src/phy_init.c
      ../../components/esp_phy/src/lib_printf.c
      )

    zephyr_link_libraries(
      net80211
      core
      pp
      coexist
      phy
      rtc
      ## esp-idf wifi libs refer gcc libs symbols, so linked in libgcc
      gcc
        -L${CMAKE_CURRENT_SOURCE_DIR}/../blobs/lib/esp32
    )

  endif()

  ## BT definitions
  if (CONFIG_BT)
    zephyr_sources(
      src/bt/esp_bt_adapter.c
      )

    zephyr_compile_definitions(
      CONFIG_BT_ENABLED
      )

    zephyr_link_libraries(
      ## ble
      btdm_app
      coexist
        -L${CMAKE_CURRENT_SOURCE_DIR}/../blobs/lib/esp32
      )

  endif()

  ## WIFI definitions
  if (CONFIG_WIFI_ESP32)
    #TODO: Additional WPA supplicant feature like Enterprise mode etc. are yet to be supported.
    set(WPA_SUPPLICANT_SRCS "src/wpa_supplicant/port/os_xtensa.c"
                       "../../components/wpa_supplicant/src/ap/ap_config.c"
                       "../../components/wpa_supplicant/src/ap/ieee802_1x.c"
                       "../../components/wpa_supplicant/src/ap/wpa_auth.c"
                       "../../components/wpa_supplicant/src/ap/wpa_auth_ie.c"
                       "../../components/wpa_supplicant/src/common/sae.c"
                       "../../components/wpa_supplicant/src/common/wpa_common.c"
                       "../../components/wpa_supplicant/src/crypto/aes-ctr.c"
                       "../../components/wpa_supplicant/src/crypto/aes-siv.c"
                       "../../components/wpa_supplicant/src/crypto/sha256-kdf.c"
                       "../../components/wpa_supplicant/src/crypto/sha256-prf.c"
                       "../../components/wpa_supplicant/src/crypto/aes-cbc.c"
                       "../../components/wpa_supplicant/src/crypto/aes-ccm.c"
                       "../../components/wpa_supplicant/src/crypto/aes-internal-dec.c"
                       "../../components/wpa_supplicant/src/crypto/aes-internal-enc.c"
                       "../../components/wpa_supplicant/src/crypto/aes-internal.c"
                       "../../components/wpa_supplicant/src/crypto/aes-omac1.c"
                       "../../components/wpa_supplicant/src/crypto/aes-unwrap.c"
                       "../../components/wpa_supplicant/src/crypto/aes-wrap.c"
                       "../../components/wpa_supplicant/src/crypto/aes-omac1.c"
                       "../../components/wpa_supplicant/src/crypto/sha256-tlsprf.c"
                       "../../components/wpa_supplicant/src/tls/bignum.c"
                       "../../components/wpa_supplicant/src/crypto/ccmp.c"
                       "../../components/wpa_supplicant/src/crypto/crypto_mbedtls-bignum.c"
                       "../../components/wpa_supplicant/src/crypto/crypto_mbedtls-ec.c"
                       "../../components/wpa_supplicant/src/crypto/crypto_ops.c"
                       "../../components/wpa_supplicant/src/crypto/crypto_internal-cipher.c"
                       "../../components/wpa_supplicant/src/crypto/crypto_internal-modexp.c"
                       "../../components/wpa_supplicant/src/crypto/crypto_internal-rsa.c"
                       "../../components/wpa_supplicant/src/crypto/crypto_internal.c"
                       "../../components/wpa_supplicant/src/crypto/des-internal.c"
                       "../../components/wpa_supplicant/src/crypto/dh_group5.c"
                       "../../components/wpa_supplicant/src/crypto/dh_groups.c"
                       "../../components/wpa_supplicant/src/crypto/md4-internal.c"
                       "../../components/wpa_supplicant/src/crypto/md5-internal.c"
                       "../../components/wpa_supplicant/src/crypto/md5.c"
                       "../../components/wpa_supplicant/src/crypto/ms_funcs.c"
                       "../../components/wpa_supplicant/src/crypto/rc4.c"
                       "../../components/wpa_supplicant/src/crypto/sha1-internal.c"
                       "../../components/wpa_supplicant/src/crypto/sha1-pbkdf2.c"
                       "../../components/wpa_supplicant/src/crypto/sha1-prf.c"
                       "../../components/wpa_supplicant/src/crypto/sha1.c"
                       "../../components/wpa_supplicant/src/crypto/sha256-internal.c"
                       "../../components/wpa_supplicant/src/crypto/sha256.c"
                       "../../components/wpa_supplicant/src/eap_peer/eap.c"
                       "../../components/wpa_supplicant/src/eap_peer/eap_common.c"
                       "../../components/wpa_supplicant/esp_supplicant/src/esp_hostap.c"
                       "../../components/wpa_supplicant/esp_supplicant/src/esp_wpa_main.c"
                       "../../components/wpa_supplicant/esp_supplicant/src/esp_wpas_glue.c"
                       "../../components/wpa_supplicant/esp_supplicant/src/esp_wpa3.c"
                       "../../components/wpa_supplicant/src/rsn_supp/pmksa_cache.c"
                       "../../components/wpa_supplicant/src/rsn_supp/wpa.c"
                       "../../components/wpa_supplicant/src/rsn_supp/wpa_ie.c"
                       "../../components/wpa_supplicant/src/utils/common.c"
                       "../../components/wpa_supplicant/src/utils/ext_password.c"
                       "../../components/wpa_supplicant/src/utils/uuid.c"
                       "../../components/wpa_supplicant/src/utils/wpabuf.c"
                       "../../components/wpa_supplicant/src/utils/wpa_debug.c"
                       "../../components/wpa_supplicant/src/utils/json.c"
                       )

    zephyr_sources(
      src/wifi/esp_wifi_adapter.c
      ${WPA_SUPPLICANT_SRCS}
      )

    zephyr_compile_definitions(
      __ets__
      ESP_SUPPLICANT
      IEEE8021X_EAPOL
      EAP_PEER_METHOD
      EAP_MSCHAPv2
      EAP_TTLS
      EAP_TLS
      EAP_PEAP
      EAP_FAST
      USE_WPA2_TASK
      ESPRESSIF_USE
      ESP32_WORKAROUND
      CONFIG_ECC
      CONFIG_IEEE80211W
      CONFIG_SHA256
      CONFIG_DPP
      CONFIG_WNM
      )

    zephyr_link_libraries(
      ## wifi
      mbedTLS
      )

  endif()

  ## Ethernet MAC/PHY definitions
  if(CONFIG_ESP32_EMAC)
    zephyr_sources(../../components/hal/emac_hal.c)
  endif()

  zephyr_link_libraries_ifdef(CONFIG_NEWLIB_LIBC c)

endif()

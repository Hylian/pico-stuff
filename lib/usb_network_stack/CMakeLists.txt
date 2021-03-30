cmake_minimum_required(VERSION 3.12)

add_library(
  tinyusb_net STATIC

  ${PICO_SDK_PATH}/lib/tinyusb/lib/networking/dhserver.c
  ${PICO_SDK_PATH}/lib/tinyusb/lib/networking/dnserver.c
  ${PICO_SDK_PATH}/lib/tinyusb/lib/networking/rndis_reports.c
)

target_include_directories(tinyusb_net PUBLIC
  ${PICO_SDK_PATH}/lib/tinyusb/lib/networking
  ${PICO_SDK_PATH}/lib/tinyusb/src
  ${PICO_EXTRAS_PATH}/lib/lwip/src/include
  ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(tinyusb_net
    lwip
    pico_stdlib
    pico_stdio
    tinyusb_host
    tinyusb_board
)
target_compile_definitions(tinyusb_net INTERFACE
    CFG_TUSB_MCU=OPT_MCU_RP2040
    CFG_TUSB_OS=OPT_OS_PICO #seems examples are hard coded to OPT_OS_NONE
    CFG_TUSB_DEBUG=${TINYUSB_DEBUG_LEVEL}
)

add_library(usb_network_stack usb_descriptors.c)

target_link_libraries(usb_network_stack
    pico_stdlib
    pico_stdio
    tinyusb_host
    tinyusb_board
    hardware_adc
    lwip
    tinyusb_net
)
target_compile_definitions(usb_network_stack INTERFACE
    CFG_TUSB_MCU=OPT_MCU_RP2040
    CFG_TUSB_OS=OPT_OS_PICO #seems examples are hard coded to OPT_OS_NONE
    CFG_TUSB_DEBUG=${TINYUSB_DEBUG_LEVEL}
)

target_include_directories(usb_network_stack PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
    ${PICO_SDK_PATH}/lib/tinyusb/lib/networking
    ${PICO_SDK_PATH}/lib/tinyusb/src/class/net
)

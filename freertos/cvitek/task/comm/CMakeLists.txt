file(GLOB _SOURCES "src/${RUN_ARCH}/*.c")

if (CONFIG_FAST_IMAGE_TYPE STRGREATER "0")
add_compile_definitions(FAST_IMAGE_ENABLE)
endif()

include_directories(include)

if (CONFIG_BOARD MATCHES "_duo256m_")
    list(APPEND _SOURCES "led_control/milkv_duo256m_io.c")
elseif (CONFIG_BOARD MATCHES "_duos_")
    list(APPEND _SOURCES "led_control/milkv_duos_io.c")
elseif (CONFIG_BOARD MATCHES "_duo_") # Check for the general 'duo' last
    list(APPEND _SOURCES "led_control/milkv_duo_io.c")
else()
    message(FATAL_ERROR "Could not determine specific Milk-V Duo board type from CONFIG_BOARD='${CONFIG_BOARD}'. No specific led_control source file added.")
endif()



include_directories(${CMAKE_INSTALL_INC_PREFIX}/arch)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/common)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/kernel)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/driver/spinlock)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/driver/jenc)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/driver/rtos_cmdqu)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/driver/fast_image)
include_directories(${CMAKE_INSTALL_INC_PREFIX}/hal/config)

add_library(comm STATIC ${_SOURCES})
install(TARGETS comm DESTINATION lib)

cmake_minimum_required(VERSION 3.10)
project(hamsterbear_native C)

set(CMAKE_C_STANDARD 11)

set(CROSS_COMPILE 0)

if(CROSS_COMPILE)
    set(CMAKE_SYSTEM_NAME linux)
    set(TOOLCHAIN_DIR "/opt/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabi/")
    set(CMAKE_C_COMPILER ${TOOLCHAIN_DIR}/bin/arm-linux-gnueabi-gcc)
    set(CMAKE_CXX_COMPILER ${TOOLCHAIN_DIR}/bin/arm-linux-gnueabi-g++)
    set(CMAKE_FIND_ROOT_PATH ${TOOLCHAIN_DIR}
            ${TOOLCHAIN_DIR}/arm-linux-gnueabi/include
            ${TOOLCHAIN_DIR}/arm-linux-gnueabi/lib)
    set(CMAKE_C_FLAGS "-fPIC")
endif(CROSS_COMPILE)

EXECUTE_PROCESS(COMMAND pkg-config --cflags gtk+-3.0
        TIMEOUT 5
        OUTPUT_VARIABLE CFLAGS
        OUTPUT_STRIP_TRAILING_WHITESPACE
        )
MESSAGE(STATUS "${CFLAGS}")

EXECUTE_PROCESS(COMMAND pkg-config --libs gtk+-3.0
        TIMEOUT 5
        OUTPUT_VARIABLE LDFLAGS
        OUTPUT_STRIP_TRAILING_WHITESPACE
        )
MESSAGE(STATUS "${LDFLAGS}")

set(CMAKE_C_FLAGS ${CFLAGS})

# add_compile_options(${CFLAGS})
link_libraries(${LDFLAGS})

add_executable(ui "src/gui/ui_main.c")

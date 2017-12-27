#
# Simple CMake file for MarlinKimbra K40 firmware
#
# Use with BareMetal library's Arduino CMake project:
#	mkdir build
#	cd build
#	cmake -DCMAKE_TOOLCHAIN_FILE=../../../../../BareMetal/Arduino/cmake/toolchain-gcc-avr-Arduino.cmake -DARD_BOARD=mega2560 ../MK4duo/
#	make
#

cmake_minimum_required(VERSION 2.8)

SET(this_target MK4duo)
PROJECT(${this_target} C CXX ASM)

set(ARD_BOARD "mega2560" CACHE STRING "Board Type")

file(GLOB_RECURSE SOURCE_FILES "src/*.cpp" "src/*.c")
file(GLOB_RECURSE HEADER_FILES "src/*.h")

if (${ARDUINO_PLATFORM} STREQUAL "AVR")
	set(ARDUINO_CPP_FLAGS "--verbose -fshort-enums")
elseif ((${ARDUINO_PLATFORM} STREQUAL "SAM") OR (${ARDUINO_PLATFORM} STREQUAL "SAMD"))
	set(ARDUINO_CPP_FLAGS "-Wa,-adhlns=\$(basename \$@).lst --verbose -fshort-enums")
endif()
set(ARDUINO_C_FLAGS "")
set(ARDUINO_CXX_FLAGS "-fno-threadsafe-statics -std=gnu++11")
if (${ARDUINO_PLATFORM} STREQUAL "AVR")
#	set(ARDUINO_LINKER_FLAGS "--verbose -Wl,-u,vfprintf -lprintf_flt -Wl,-u,vfscanf -lscanf_flt")
	set(ARDUINO_LINKER_FLAGS "--verbose")
elseif ((${ARDUINO_PLATFORM} STREQUAL "SAM") OR (${ARDUINO_PLATFORM} STREQUAL "SAMD"))
	set(ARDUINO_LINKER_FLAGS "--verbose")
endif()

generate_arduino_firmware(${this_target}
	SKETCH "${this_target}.ino"
	SRCS ${SOURCE_FILES}
	HDRS ${HEADER_FILES}
)

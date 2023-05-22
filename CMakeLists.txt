# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)
set(PICO_SDK_PATH "/home/pi/pico/pico-sdk")

set(PICO_BOARD pico CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

if (PICO_SDK_VERSION_STRING VERSION_LESS "1.4.0")
  message(FATAL_ERROR "Raspberry Pi Pico SDK version 1.4.0 (or later) required. Your version is ${PICO_SDK_VERSION_STRING}")
endif()

project(AcadoPi C CXX)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(AcadoPi AcadoPi.cpp )

pico_set_program_name(AcadoPi "AcadoPi")
pico_set_program_version(AcadoPi "0.1")

pico_enable_stdio_usb(AcadoPi 1)
pico_enable_stdio_uart(AcadoPi 0)

include_directories(
    qpoases
    qpoases/INCLUDE
    qpoases/SRC
    acado_solver)


# qpoases
FILE( GLOB LIB_SOURCES qpoases/SRC/*.cpp )
FILE( GLOB LIB_HEADERS qpoases/INCLUDE/*.hpp )
set_source_files_properties(${LIB_SOURCES1} PROPERTIES LANGUAGE CXX )
ADD_LIBRARY( qpoases ${LIB_SOURCES} ${LIB_HEADERS} )

# acado_solver
FILE( GLOB LIB_SOURCES acado_solver/*.cpp acado_solver/*.c )
FILE( GLOB LIB_HEADERS acado_solver/*.hpp acado_solver/*.h )
set_source_files_properties(${LIB_SOURCES} PROPERTIES LANGUAGE CXX )
ADD_LIBRARY( acado_solver ${LIB_SOURCES} ${LIB_HEADERS} )
target_link_libraries( acado_solver qpoases )


# Add the standard library to the build
target_link_libraries(AcadoPi
        pico_stdlib )

# Add the standard include files to the build
target_include_directories(AcadoPi PRIVATE
  ${CMAKE_CURRENT_LIST_DIR}
  ${CMAKE_CURRENT_LIST_DIR}/.. # for our common lwipopts or any other standard includes, if required
)

# Add any user requested libraries
target_link_libraries(AcadoPi 
        acado_solver qpoases )

pico_add_extra_outputs(AcadoPi)


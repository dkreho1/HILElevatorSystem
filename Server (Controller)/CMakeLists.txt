# Copyright (c) 2023 ARM Limited. All rights reserved.
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.19.0)

set(MBED_PATH ${CMAKE_CURRENT_SOURCE_DIR}/mbed-os CACHE INTERNAL "")
set(MBED_CONFIG_PATH ${CMAKE_CURRENT_BINARY_DIR} CACHE INTERNAL "")
set(APP_TARGET Server)
#add_compile_definitions(NDEBUG)
add_definitions("-fexceptions") # no need to add "-frtti" but remove "-fno-rtti" from cmake files in mbed-os/tools/cmake/profiles 


set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(JSONRPCPP_PATH ${CMAKE_CURRENT_SOURCE_DIR}/jsonrpcpp/include CACHE INTERNAL "")


include(${MBED_PATH}/tools/cmake/app.cmake)


project(${APP_TARGET})


add_subdirectory(${MBED_PATH})

include_directories(include
					${JSONRPCPP_PATH})
					
set(SOURCES src/Elevator.cpp
			src/ServerComModule.cpp
			src/ServerController.cpp)


add_executable(${APP_TARGET}
    src/main.cpp
    ${SOURCES}
)

target_link_libraries(${APP_TARGET} mbed-os)


mbed_set_post_build(${APP_TARGET})

option(VERBOSE_BUILD "Have a verbose build process")
if(VERBOSE_BUILD)
    set(CMAKE_VERBOSE_MAKEFILE ON)
endif()

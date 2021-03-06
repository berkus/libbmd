cmake_minimum_required(VERSION 2.8)

set(CMAKE_BUILD_TYPE Debug)
project (libbmd)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake-modules)

# configure_file(libbmd.pc.in ${OUTPUT_DIR}/libbmd.pc)

option(SDK_DIR "SDK directory path" "")
option(BUILD_TOOLS "Build various tools" OFF)

include_directories(${SDK_DIR})
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-rtti -Wno-multichar -D__STDC_CONSTANT_MACROS")

set(CMAKE_THREAD_PREFER_PTHREAD ON)
find_package(Threads REQUIRED)

add_library(bmd
    src/decklink_capture.cpp)

if (BUILD_TOOLS)
    find_package(LibAV REQUIRED)
    include_directories(${LIBAV_INCLUDE_DIRS})

    set(CMAKE_LINK_FLAGS "${CMAKE_LINK_FLAGS} -fPIC")

    #add_executable(bmdplay src/bmdplay.cpp)
    #target_link_libraries(bmdplay bmd )

    add_executable(bmdcapture  src/bmdcapture.c)
    target_link_libraries(bmdcapture ${LIBAV_LIBRARIES} bmd ${CMAKE_THREAD_LIBS_INIT} dl)

#add_executable(bmdgenlock src/bmdgenlock.cpp)
#target_link_libraries(bmdgenlock bmd)

endif (BUILD_TOOLS)


cmake_minimum_required(VERSION 3.0.0)
  
project(ffmpeg-cpp CXX)
  
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()
  
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_VERBOSE_MAKEFILE OFF)
set(CMAKE_CXX_FLAGS "-D__STDC_FORMAT_MACROS")

if (CMAKE_SYSTEM MATCHES Linux)
#testing some warning flags. Be carefull: some can be DANGEROUS when badly used
   set(CMAKE_CXX_FLAGS "-g -DDEBUG -DLinux -Wall -Wextra -Wno-unused-parameter -Wno-unused-function -Wunused-variable -Wunused-value -Wshadow -Wcast-qual -Wcast-align -Wwrite-strings -Wsign-compare -fms-extensions -Wfloat-equal")
##    set(CMAKE_CXX_FLAGS "-Wall -Wextra -g -DDEBUG -DLinux")
endif()

file(GLOB_RECURSE INC sources/*.h)
file(GLOB_RECURSE SRC sources/*.cpp)

add_library(${PROJECT_NAME} ${SRC}) 

target_compile_options(${PROJECT_NAME} PUBLIC -D__STDC_FORMAT_MACROS)
target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/sources)

# Find external FFmpeg libraries.
find_path(AVCODEC_INCLUDE_DIR libavcodec/avcodec.h)
find_library(AVCODEC_LIBRARY avcodec)

find_path(AVFORMAT_INCLUDE_DIR libavformat/avformat.h)
find_library(AVFORMAT_LIBRARY avformat)

find_path(AVUTIL_INCLUDE_DIR libavutil/avutil.h)
find_library(AVUTIL_LIBRARY avutil)

find_path(AVDEVICE_INCLUDE_DIR libavdevice/avdevice.h)
find_library(AVDEVICE_LIBRARY avdevice)

find_path(AVFILTER_INCLUDE_DIR libavfilter/avfilter.h)
find_library(AVFILTER_LIBRARY avfilter)

find_path(SWRESAMPLE_INCLUDE_DIR libswresample/swresample.h)
find_library(SWRESAMPLE_LIBRARY swresample)

find_path(SWSCALE_INCLUDE_DIR libswscale/swscale.h)
find_library(SWSCALE_LIBRARY swscale)

target_include_directories(${PROJECT_NAME} PRIVATE ${AVCODEC_INCLUDE_DIR} ${AVFORMAT_INCLUDE_DIR} ${AVUTIL_INCLUDE_DIR} ${AVDEVICE_INCLUDE_DIR} ${AVFILTER_INCLUDE_DIR} ${SWRESAMPLE_INCLUDE_DIR} ${SWSCALE_INCLUDE_DIR})
target_link_libraries(${PROJECT_NAME} PRIVATE ${AVCODEC_LIBRARY} ${AVFORMAT_LIBRARY} ${AVUTIL_LIBRARY} ${AVDEVICE_LIBRARY} ${AVFILTER_LIBRARY} ${SWRESAMPLE_LIBRARY} ${SWSCALE_LIBRARY})
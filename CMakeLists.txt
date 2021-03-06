cmake_minimum_required(VERSION 2.8.1)
project(mumsi)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

INCLUDE(FindPkgConfig)
find_package(Boost COMPONENTS system regex REQUIRED)
find_package(PkgConfig REQUIRED)

pkg_check_modules(PJSIP "libpjproject")
pkg_check_modules(LOG4CPP "log4cpp")

add_definitions(${PJSIP_CFLAGS})
add_definitions(${LOG4CPP_CFLAGS})

include_directories(${PJSIP_INCLUDE_DIRS})
include_directories(${LOG4CPP_INCLUDE_DIRS})
include_directories(../mumlib/include)

link_directories(../mumlib/build)

set(SOURCE_FILES
        PjsuaCommunicator.cpp
        PjsuaCommunicator.hpp
        MumbleCommunicator.cpp
        MumbleCommunicator.hpp
        Configuration.cpp
        Configuration.hpp
        IncomingConnectionValidator.cpp
        IncomingConnectionValidator.hpp
        AudioFramesMixer.hpp
        AudioFramesMixer.cpp)

add_executable(mumsi ${SOURCE_FILES} main.cpp)
target_link_libraries(mumsi ${PJSIP_LIBRARIES})
target_link_libraries(mumsi ${LOG4CPP_LIBRARIES})
target_link_libraries(mumsi ${Boost_LIBRARIES})
target_link_libraries(mumsi mumlib)


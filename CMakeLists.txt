cmake_minimum_required(VERSION 3.30)

# Vcpkg toolchain
set(CMAKE_TOOLCHAIN_FILE "${CMAKE_CURRENT_SOURCE_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake"
    CACHE STRING "Vcpkg toolchain file")

project(SmartTrails VERSION 1.0)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_SCAN_FOR_MODULES OFF)

find_package(PostgreSQL REQUIRED)

find_package(Poco REQUIRED COMPONENTS
    NetSSL
    Net
    Crypto
    Util
    Data
    DataPostgreSQL
    JSON
)

file(GLOB_RECURSE SmartTrails_SOURCES CONFIGURE_DEPENDS 
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.h"
)

add_executable(SmartTrails ${SmartTrails_SOURCES})
target_include_directories(SmartTrails PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)

target_link_libraries(SmartTrails PRIVATE
    PostgreSQL::PostgreSQL 
    Poco::NetSSL
    Poco::Net
    Poco::Crypto
    Poco::Util
    Poco::Data
    Poco::DataPostgreSQL
    Poco::JSON
)

if(MSVC)
    target_compile_options(SmartTrails PRIVATE /W3 /MP /WX /ZI)
else()
    target_compile_options(SmartTrails PRIVATE -Wall -Wextra -Wpedantic)
endif()

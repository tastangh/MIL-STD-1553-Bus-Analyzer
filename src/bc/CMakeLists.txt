cmake_minimum_required(VERSION 3.21)
project(bus-controller LANGUAGES C CXX)

# Set target directory
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/../../bin/)

# Find boost installation
find_package(Boost 1.74.0 COMPONENTS filesystem REQUIRED)

# Find spdlog installation
find_package(spdlog REQUIRED)

add_definitions(-DBOOST_BIND_GLOBAL_PLACEHOLDERS)

# Include SourceFiles.cmake to access the SOURCEFILES variable
include(${CMAKE_CURRENT_LIST_DIR}/SourceFiles.cmake)

add_executable(bus-controller ${SOURCEFILES})

target_compile_definitions(bus-controller PUBLIC
    _FILE_OFFSET_BITS=64 WXUSINGDLL __WXGTK__ _AIM_LINUX
)

target_link_libraries(bus-controller PRIVATE
    Boost::filesystem
    spdlog::spdlog
    ${CMAKE_CURRENT_LIST_DIR}/../../deps/wxWidgets/lib/libwx_baseu_net-3.2.so.0
    ${CMAKE_CURRENT_LIST_DIR}/../../deps/wxWidgets/lib/libwx_baseu-3.2.so.0
    ${CMAKE_CURRENT_LIST_DIR}/../../deps/wxWidgets/lib/libwx_gtk3u_core-3.2.so.0
    ${CMAKE_CURRENT_LIST_DIR}/../../deps/aim-driver/lib/libaim_mil.so

)

target_include_directories(bus-controller PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}
    ${CMAKE_CURRENT_LIST_DIR}/ui
    ${CMAKE_CURRENT_LIST_DIR}/../
    ${CMAKE_CURRENT_LIST_DIR}/../../deps/aim-driver/include/aim_mil_24.22
    ${CMAKE_CURRENT_LIST_DIR}/../../deps/wxWidgets/include
    ${CMAKE_CURRENT_LIST_DIR}/../../deps/wxWidgets/lib/wx/include/gtk3-unicode-3.2
)
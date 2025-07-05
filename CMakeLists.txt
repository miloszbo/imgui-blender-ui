cmake_minimum_required(VERSION 3.15)

project(ImGuiBlenderUI)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(Vulkan REQUIRED)

message(STATUS "VULKAN_SDK = $ENV{VULKAN_SDK}")

set(IMGUI_SOURCES
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/imgui.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/imgui_demo.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/imgui_draw.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/imgui_tables.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/imgui_widgets.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/backends/imgui_impl_glfw.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/backends/imgui_impl_vulkan.cpp
)

add_library(imgui ${IMGUI_SOURCES})

target_include_directories(imgui PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/imgui
                                        ${CMAKE_CURRENT_SOURCE_DIR}/imgui/examples/libs/glfw/include
                                        "$ENV{VULKAN_SDK}/Include"
                                    )

add_executable(example example/main.cpp)

target_include_directories(example PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/imgui
                                        ${CMAKE_CURRENT_SOURCE_DIR}/imgui/examples/libs/glfw/include
                                        "$ENV{VULKAN_SDK}/Include"
                                    )

target_link_directories(example PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/examples/libs/glfw/lib-vc2010-64)

target_link_directories(example PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/imgui/examples/libs/glfw/lib-vc2010-64)


target_link_libraries(example PUBLIC glfw3 imgui Vulkan::Vulkan)
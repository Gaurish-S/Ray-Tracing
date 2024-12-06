cmake_minimum_required(VERSION 3.10)
project(MyImGuiApp)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set the full path to ImGui directory
set(IMGUI_DIR "${CMAKE_CURRENT_SOURCE_DIR}/external/imgui")

# Add ImGui source files (manual inclusion)
set(IMGUI_SOURCES
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
)

# Create ImGui library
add_library(imgui STATIC ${IMGUI_SOURCES})

# Find OpenGL
find_package(OpenGL REQUIRED)

# Find GLFW
find_package(glfw3 REQUIRED)

# Add your application source files
file(GLOB SOURCES "src/*.cpp")
add_executable(MyImGuiApp ${SOURCES})

# Include ImGui headers
# Use SYSTEM to treat as system headers 
target_include_directories(imgui SYSTEM PUBLIC 
    ${IMGUI_DIR}
    ${IMGUI_DIR}/backends
)

# Also include ImGui headers for the main application
target_include_directories(MyImGuiApp PRIVATE
    ${IMGUI_DIR}
    ${IMGUI_DIR}/backends
)

# Link ImGui, OpenGL, and GLFW libraries
target_link_libraries(MyImGuiApp PRIVATE imgui OpenGL::GL glfw)
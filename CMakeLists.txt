cmake_minimum_required(VERSION 3.10)
project(MyImGuiApp)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set the full path to ImGui directory
set(IMGUI_DIR "${CMAKE_CURRENT_SOURCE_DIR}/external/imgui")

# Comprehensive ImGui source files
set(IMGUI_SOURCES
    # Core ImGui sources
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    
    ${IMGUI_DIR}/imgui_demo.cpp
    
    # Backend sources
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
)

# Create ImGui library
add_library(imgui STATIC ${IMGUI_SOURCES})

set(OpenGL_GL_PREFERENCE GLVND)

# Find necessary packages
find_package(OpenGL REQUIRED)
find_package(glfw3 REQUIRED)
find_package(GLEW REQUIRED)

# Add your application source files
file(GLOB SOURCES "src/*.cpp")

# Create executable
add_executable(MyImGuiApp ${SOURCES})

# Include ImGui headers
target_include_directories(imgui PUBLIC
    ${IMGUI_DIR}
    ${IMGUI_DIR}/backends
)

# Include ImGui headers for the main application
target_include_directories(MyImGuiApp PRIVATE
    ${IMGUI_DIR}
    ${IMGUI_DIR}/backends
)

# Add ImGui definitions
add_definitions(-DIMGUI_IMPL_OPENGL_LOADER_GLEW)

# Link libraries
target_link_libraries(MyImGuiApp PRIVATE 
    imgui 
    OpenGL::GL 
    glfw 
    GLEW::GLEW
)
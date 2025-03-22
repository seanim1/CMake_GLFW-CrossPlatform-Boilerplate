cmake_minimum_required(VERSION 3.20)
project(GLFWExample)

if(WIN32)
    # Specify the path to the GLFW installation directory in vcpkg
    set(CMAKE_PREFIX_PATH "C:/vcpkg/installed/x64-windows/share/glfw3" CACHE STRING "Path to glfw3Targets.cmake, glfw3Config.cmake, etc")
endif()

# Find <PackageName>.cmake in the locations specified by CMAKE_PREFIX_PATH
# Windows: You can find this information in .vcxproj at <AdditionalInputs Condition="'$(Configuration)|$(Platform)'
find_package(GLFW3 REQUIRED)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 11)

# Add the main.cpp file to the project
add_executable(GLFWExample main.cpp)

# On Windows, ensure the glfw3.dll is copied to the output directory
if(WIN32)
    # The path to your glfw3.dll file
    set(GLFW_DLL_PATH "C:/vcpkg/installed/x64-windows/bin/glfw3.dll")

    # Copy the glfw3.dll file to the output directory after build.
    # You can find this information in .vcxproj at <PostBuildEvent>
    add_custom_command(TARGET GLFWExample POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${GLFW_DLL_PATH}"
        $<TARGET_FILE_DIR:GLFWExample>/glfw3.dll)
endif()

# Link GLFW with your project
target_link_libraries(GLFWExample glfw)

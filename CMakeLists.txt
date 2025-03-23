cmake_minimum_required(VERSION 3.20)
project(GLFWExample)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 14)

# Check platform
if(WIN32)
    # Specify the path to the GLFW installation directory in vcpkg

    # Find GLFW using FindGLFW.cmake
    # This probably looks for the include file: 'GLFW/glfw3.h'
    find_package(GLFW3 REQUIRED)

    # The path to your glfw3.dll file
    set(GLFW_DLL_PATH "C:/vcpkg/installed/x64-windows/bin/glfw3.dll")

else() # Linux/macOS

    # It adds "${CMAKE_SOURCE_DIR}" to CMAKE_MODULE_PATH, allowing CMake to search for .cmake files in the source directory
    # Make sure CMake finds FindGLFW.cmake
    list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}")

    # When find_package(GLFW REQUIRED) is called:
    #   1. CMake first looks in CMAKE_MODULE_PATH for FindGLFW.cmake
    #   2. If found, it runs FindGLFW.cmake, which uses find_path() and find_library() to locate GLFW.
    # Find GLFW using FindGLFW.cmake
    find_package(GLFW3 REQUIRED)

endif()

# Add the main.cpp file to the project
add_executable(GLFWExample main.cpp)


# Only add post-build commands after the target is created
if(WIN32)
    # Link GLFW with your project
    target_link_libraries(GLFWExample glfw)

    add_custom_command(TARGET GLFWExample POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${GLFW_DLL_PATH}"
        $<TARGET_FILE_DIR:GLFWExample>/glfw3.dll)
else()
    # Include GLFW headers
    include_directories(${GLFW_INCLUDE_DIR})

    # Link GLFW with your project
    target_link_libraries(GLFWExample ${GLFW_LIBRARY})

    add_custom_command(TARGET GLFWExample POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${GLFW_LIBRARY}"
        $<TARGET_FILE_DIR:GLFWExample>/libglfw.so)
endif()


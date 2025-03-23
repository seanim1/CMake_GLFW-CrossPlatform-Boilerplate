cmake_minimum_required(VERSION 3.20)
project(PROJECT_NAME)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 14)

# Check platform
if(WIN32)
    # Specify the path to the GLFW installation directory in vcpkg
    set(CMAKE_PREFIX_PATH "C:/vcpkg/installed/x64-windows/share/glfw3" CACHE STRING "Path to glfw3Targets.cmake, glfw3Config.cmake, etc")

    # If CMAKE_PREFIX_PATH is set:
    #   CMake will attempt to find GLFW3Config.cmake or glfw3-config.cmake in the subdirectories of CMAKE_PREFIX_PATH
    # looks like "GLFW3" only works on WIN32, and "GLFW" only works on Linux...
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
    find_package(GLFW REQUIRED)
endif()

# Add the main.cpp file to the project
add_executable(EXECUTABLE_TARGET_NAME main.cpp)


# Only add post-build commands after the target is created
if(WIN32)
    # Link GLFW with your project
    target_link_libraries(EXECUTABLE_TARGET_NAME glfw)

    add_custom_command(TARGET EXECUTABLE_TARGET_NAME POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${GLFW_DLL_PATH}"
        $<TARGET_FILE_DIR:EXECUTABLE_TARGET_NAME>/glfw3.dll)
else()
    # Include GLFW headers
    include_directories(${GLFW_INCLUDE_DIR})

    # Link GLFW with your project
    target_link_libraries(EXECUTABLE_TARGET_NAME ${GLFW_LIBRARY})

    add_custom_command(TARGET EXECUTABLE_TARGET_NAME POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${GLFW_LIBRARY}"
        $<TARGET_FILE_DIR:EXECUTABLE_TARGET_NAME>/libglfw.so)
endif()


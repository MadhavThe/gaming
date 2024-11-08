cmake_minimum_required(VERSION 3.15)
project(GAME)

# Set the C++ standard (optional, SDL2 works with C++11 and later)
set(CMAKE_CXX_STANDARD 17)

# Define the target triplet and vcpkg installation directory
set(VCPKG_TARGET_TRIPLET "x64-windows") # Change if targeting another platform
set(CMAKE_PREFIX_PATH "D:/GAME/vcpkg/installed/${VCPKG_TARGET_TRIPLET}")

# Uncomment and adjust the following line if you haven't set vcpkg as a global CMake package manager
set(CMAKE_TOOLCHAIN_FILE "D:/GAME/vcpkg/scripts/buildsystems/vcpkg.cmake" CACHE STRING "")

# Find SDL2 package using vcpkg
find_package(SDL2 CONFIG REQUIRED)

# Add the executable
add_executable(Game D:/game/src/main.cpp)

# Link SDL2 to the target
target_link_libraries(Game PRIVATE SDL2::SDL2 SDL2::SDL2main)

# Copy SDL2 DLLs for Windows runtime (optional)
if(WIN32)
    add_custom_command(TARGET Game POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
            "${CMAKE_PREFIX_PATH}/bin" $<TARGET_FILE_DIR:Game>)
endif()
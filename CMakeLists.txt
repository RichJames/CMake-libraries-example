cmake_minimum_required(VERSION 3.13.4)

# Set target output directory
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")

# Add BUILD_SHARED_LIBS to the CMake cache
option(BUILD_SHARED_LIBS "True when building a shared library" True)

project("Foo")

# The add_subdirectory command tells CMake to go into the named subdirectory and execute
# the CMakeLists.txt file in that directory.
add_subdirectory("foo")

add_subdirectory("examples")


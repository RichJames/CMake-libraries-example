# The below variable can be set to control whether to build shared libraries or not.  Set to true
# to build shared libraries.
#BUILD_SHARED_LIBS 

# The 2nd parameter says what source files are used to build the target named in the 1st param.
# In this case, we are not naming the source files.
# You can say in this command if you want to build a static or shared library, like so:
# add_library(target SHARED src_files)
# add_library(target STATIC src_files)
# In this case, we are using the BUILD_SHARED_LIBS variable to set this (see the line above
# this section).
add_library("${PROJECT_NAME}" "")
add_library(Foo::Foo ALIAS "${PROJECT_NAME}")

# To specify the include directories, use the target_include_directories command:
# CMAKE_CURRENT_SOURCE_DIR means this directory.
# The way the generator works ($<>) is, when we are in the build directory, that string
# evaluates to the path named after the ':'.
# When not in the build directory, it evaluates to nothing.
# The install interface: since install is a relative path, CMake will add CMAKE_INSTALL_PREFIX
# to the beginning.
target_include_directories("${PROJECT_NAME}" PUBLIC
	"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"	
	"$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>" # foo_export.h
	"$<INSTALL_INTERFACE:include>"	# ${CMAKE_INSTALL_PREFIX}/include
	)

# CMake has build requirements and usage requirements.  If we mark something as PRIVATE,
# it is a build requirement.  If we mark it as INTERFACE, it is a usage requirement.
# IF we mark it as PUBLIC, it is both a build and a usage requirement.
# Build requirements are anything needed to compile a target.
# Usage requirements are anything needed to link a target.
#
# Therefore, the target_include_directories are named PUBLIC because they are needed to both
# compile and link the target.

# We want to rename our target.  If we build a debug version, it should be named foo-d.  If we
# build a static debug version, it should be named foo-s-d.
if(NOT BUILD_SHARED_LIBS)
	set(STATIC_POSTFIX "-s")
endif()

set_target_properties("${PROJECT_NAME}" PROPERTIES
	DEBUG_POSTFIX "${STATIC_POSTFIX}-d"
	RELEASE_POSTFIX "${STATIC_POSTFIX}"
	MINSIZEREL_POSTFIX "${STATIC_POSTFIX}-mr"
	RELWITHDEBINFO_POSTFIX "${STATIC_POSTFIX}-rd"
	)

include("GenerateExportHeader")
generate_export_header("${PROJECT_NAME}")
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/foo_export.h" DESTINATION "include")

add_subdirectory("include")
add_subdirectory("src")

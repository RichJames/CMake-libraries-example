# Must be an absolute path
set(FILES "${CMAKE_CURRENT_SOURCE_DIR}/hello.cpp")
set(SUBDIRECTORIES "")
set(RELATIVE_PATH "src/foo")

# Add all subdirectories
foreach(VAR ${SUBDIRECTORIES})
	add_subdirectory(${VAR})
endforeach()

# check for files inside this directory
if(FILES)
	# Add source files to target
	target_sources("${PROJECT_NAME}" PRIVATE ${FILES})
	
	# Install our source files (to ${CMAKE_INSTALL_PREFIX}/include)
	install(FILES ${FILES} DESTINATION ${RELATIVE_PATH})
endif()

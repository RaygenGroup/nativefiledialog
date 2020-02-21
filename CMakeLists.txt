cmake_minimum_required(VERSION 3.11)


project (NativeFileDialog)



file(GLOB_RECURSE HEADER_FILES CONFIGURE_DEPENDS "*.h")

if(WIN32)
	set(OS_SRC
		src/nfd_win.cpp
	)
endif()

if(APPLE)
	set(OS_SRC
		src/nfd_cocoa.m
	)    
endif()

if(UNIX AND NOT APPLE)
	set(OS_SRC
		src/nfd_gtk.c
	)    
endif()

set(SRC 
	src/nfd_common.c
)

add_library(NativeFileDialog ${SRC} ${OS_SRC} ${HEADER_FILES})

target_include_directories(NativeFileDialog PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)
target_include_directories(NativeFileDialog PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src/include/)

if(MSVC)
	target_compile_options(NativeFileDialog PRIVATE "/MP")
	target_compile_options(NativeFileDialog PRIVATE "/D_CRT_SECURE_NO_WARNINGS")
endif()

# enforce cpp20
set_property(TARGET NativeFileDialog PROPERTY CXX_STANDARD 20)
set_property(TARGET NativeFileDialog PROPERTY CXX_STANDARD_REQUIRED ON)

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} PREFIX ${PROJECT_NAME} FILES ${SRC} ${OS_SRC} ${HEADER_FILES})

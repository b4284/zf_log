cmake_minimum_required(VERSION 3.2)

# zf_log target (required)
set(HEADERS_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(HEADERS zf_log.h)
set(SOURCES zf_log.c)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -Wall -Wextra -Werror -pedantic-errors")

add_library(zf_log ${HEADERS} ${SOURCES})
target_include_directories(zf_log PUBLIC $<BUILD_INTERFACE:${HEADERS_DIR}>)
if(ZF_LOG_ANDROID_LOG)
	target_compile_definitions(zf_log PRIVATE "ZF_LOG_ANDROID_LOG=1") 
	target_link_libraries(zf_log log)
endif()
if(ZF_LOG_APPLE_LOG)
	target_compile_definitions(zf_log PRIVATE "ZF_LOG_APPLE_LOG=1") 
	target_link_libraries(zf_log "-framework CoreFoundation")
endif()

# install (optional)
if(ZF_LOG_CONFIGURE_INSTALL)
	if(NOT DEFINED INSTALL_INCLUDE_DIR)
		set(INSTALL_INCLUDE_DIR include)
	endif()
	if(NOT DEFINED INSTALL_LIB_DIR)
		set(INSTALL_LIB_DIR lib)
	endif()
	install(TARGETS zf_log EXPORT zf_log
		INCLUDES DESTINATION ${INSTALL_INCLUDE_DIR}
		ARCHIVE DESTINATION ${INSTALL_LIB_DIR})
	install(DIRECTORY ${HEADERS_DIR}/
		DESTINATION ${INSTALL_INCLUDE_DIR}
		FILES_MATCHING PATTERN "zf_*.h*")
endif()

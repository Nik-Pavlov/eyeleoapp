cmake_minimum_required(VERSION 3.2)

add_subdirectory("libs/pugixml")

project(EyeLeo VERSION 1.3.0)

set(SOURCE_FILES_FOLDER ${CMAKE_CURRENT_SOURCE_DIR}/source/code)

# build pugixml
#set(STATIC_CRT ON CACHE BOOL "Use static CRT" FORCE)

#
add_executable(EyeLeo ${SOURCE_FILES_FOLDER}/main.cpp)

# Common compilation defines/options
if(MSVC)
	target_compile_definitions(EyeLeo PRIVATE
		-D_CRT_SECURE_NO_WARNINGS
		-D_UNICODE
		-DUNICODE)
endif()

if(MSVC)
	target_compile_options(EyeLeo PRIVATE /W4)
else()
	target_compile_options(EyeLeo PRIVATE -Wall -Wextra -Wpedantic)
endif()

target_compile_options(EyeLeo PRIVATE /Fx)

# CRT - multithreaded debug dll
target_compile_options(EyeLeo PRIVATE /MDd)

# wxWidgets dependency
#if(NOT $ENV{WXWIN})
#	message(FATAL_ERROR "WXWIN environment variable is not set")
#endif()

target_include_directories(EyeLeo PRIVATE "$ENV{WXWIN}/include")
target_compile_definitions(EyeLeo PRIVATE
	-D__WXMSW__
	-D__WXDEBUG__
	-DwxUSE_TASKBARICON_BALLOONS)

foreach(LIB_NAME
		wxbase30ud
		wxmsw30ud_core
		wxmsw30ud_adv
		wxtiffd
		wxjpegd
		wxpngd
		wxzlibd)
	find_library(LIB_PATH ${LIB_NAME} HINTS $ENV{WXWIN}/lib/vc_lib)
	if(NOT LIB_PATH)
		message(FATAL_ERROR "Can't find wxWidgets library ${LIB_NAME}")
	endif()
	target_link_libraries(EyeLeo PRIVATE ${LIB_PATH})
	unset(LIB_PATH CACHE)
endforeach()

# PugiXml dependency
target_include_directories(EyeLeo PRIVATE "libs/pugixml/src/")
target_compile_definitions(EyeLeo PRIVATE
	-DPUGIXML_WCHAR_MODE)

link_directories(libs/pugixml/build/Debug/)
target_link_libraries(EyeLeo PRIVATE pugixml.lib)


# Source files
target_sources(EyeLeo PRIVATE 
	${SOURCE_FILES_FOLDER}/activity_monitor.cpp
	${SOURCE_FILES_FOLDER}/activity_monitor.h
	${SOURCE_FILES_FOLDER}/beforepause_wnd.cpp
	${SOURCE_FILES_FOLDER}/beforepause_wnd.h
	${SOURCE_FILES_FOLDER}/bigpause_wnd.cpp
	${SOURCE_FILES_FOLDER}/bigpause_wnd.h
	${SOURCE_FILES_FOLDER}/debug_wnd.cpp
	${SOURCE_FILES_FOLDER}/debug_wnd.h
	${SOURCE_FILES_FOLDER}/excercises.cpp
	${SOURCE_FILES_FOLDER}/excercises.h
	${SOURCE_FILES_FOLDER}/image_resources.cpp
	${SOURCE_FILES_FOLDER}/image_resources.h
	${SOURCE_FILES_FOLDER}/language_set.cpp
	${SOURCE_FILES_FOLDER}/language_set.h
	${SOURCE_FILES_FOLDER}/logging.cpp
	${SOURCE_FILES_FOLDER}/logging.h
	${SOURCE_FILES_FOLDER}/main.h
	${SOURCE_FILES_FOLDER}/minipause_wnd.cpp
	${SOURCE_FILES_FOLDER}/minipause_wnd.h
	${SOURCE_FILES_FOLDER}/notification_wnd.cpp
	${SOURCE_FILES_FOLDER}/notification_wnd.h
	${SOURCE_FILES_FOLDER}/oscapabilities.cpp
	${SOURCE_FILES_FOLDER}/oscapabilities.h
	${SOURCE_FILES_FOLDER}/settings.cpp
	${SOURCE_FILES_FOLDER}/settings.h
	${SOURCE_FILES_FOLDER}/settings_wnd.cpp
	${SOURCE_FILES_FOLDER}/settings_wnd.h
	${SOURCE_FILES_FOLDER}/task_mgr.cpp
	${SOURCE_FILES_FOLDER}/task_mgr.h
	${SOURCE_FILES_FOLDER}/timeloc.cpp
	${SOURCE_FILES_FOLDER}/timeloc.h
	${SOURCE_FILES_FOLDER}/waiting_wnd.cpp
	${SOURCE_FILES_FOLDER}/waiting_wnd.h)

target_link_libraries(EyeLeo PRIVATE
	comctl32.lib
	winmm.lib
	rpcrt4.lib
	Wtsapi32.lib)
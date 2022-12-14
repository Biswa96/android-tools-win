cmake_minimum_required(VERSION 3.15)
cmake_policy(SET CMP0091 NEW)
project(android-tools-win)

if(NOT MSVC)
    message(FATAL_ERROR "ADB API for Windows can only be built using MSVC.")
endif()

# For static linking
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

set(adbwinapi_SOURCES
api/AdbWinApi.cpp
api/AdbWinApi.def
api/AdbWinApi.rc
api/Resource.h
api/adb_api.cpp
api/adb_api.h
api/adb_api_instance.cpp
api/adb_api_instance.h
api/adb_api_legacy.h
api/adb_api_private_defines.h
api/adb_endpoint_object.cpp
api/adb_endpoint_object.h
api/adb_helper_routines.cpp
api/adb_helper_routines.h
api/adb_interface.cpp
api/adb_interface.h
api/adb_interface_enum.cpp
api/adb_interface_enum.h
api/adb_io_completion.cpp
api/adb_io_completion.h
api/adb_legacy_endpoint_object.cpp
api/adb_legacy_endpoint_object.h
api/adb_legacy_interface.cpp
api/adb_legacy_interface.h
api/adb_legacy_io_completion.cpp
api/adb_legacy_io_completion.h
api/adb_object_handle.cpp
api/adb_object_handle.h
api/adb_winusb_api.h
api/stdafx.cpp
api/stdafx.h
)

add_library(AdbWinApi SHARED ${adbwinapi_SOURCES})
target_compile_definitions(AdbWinApi PRIVATE
ADBWIN_EXPORTS _CRT_NON_CONFORMING_SWPRINTFS _CRT_SECURE_NO_WARNINGS _UNICODE UNICODE
NTDDI_VERSION=NTDDI_WIN10 _WIN32_WINNT=_WIN32_WINNT_WIN10 WINVER=_WIN32_WINNT_WIN10)
target_link_libraries(AdbWinApi PRIVATE setupapi)

set(adbwinusbapi_SOURCES
winusb/AdbWinUsbApi.cpp
winusb/AdbWinUsbApi.def
winusb/AdbWinUsbApi.rc
winusb/Resource.h
winusb/adb_winusb_endpoint_object.cpp
winusb/adb_winusb_endpoint_object.h
winusb/adb_winusb_interface.cpp
winusb/adb_winusb_interface.h
winusb/adb_winusb_io_completion.cpp
winusb/adb_winusb_io_completion.h
winusb/stdafx.cpp
winusb/stdafx.h
)

add_library(AdbWinUsbApi SHARED ${adbwinusbapi_SOURCES})
target_compile_definitions(AdbWinUsbApi PRIVATE
ADBWIN_EXPORTS _CRT_NON_CONFORMING_SWPRINTFS _CRT_SECURE_NO_WARNINGS _UNICODE UNICODE
NTDDI_VERSION=NTDDI_WIN10 _WIN32_WINNT=_WIN32_WINNT_WIN10 WINVER=_WIN32_WINNT_WIN10)
target_link_libraries(AdbWinUsbApi PRIVATE AdbWinApi winusb)

install(TARGETS AdbWinApi AdbWinUsbApi RUNTIME DESTINATION bin)

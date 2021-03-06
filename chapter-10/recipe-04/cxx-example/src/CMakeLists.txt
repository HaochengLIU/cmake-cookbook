cmake_minimum_required(VERSION 3.6 FATAL_ERROR)

project(recipe-04_core
  LANGUAGES CXX
  )

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(GNUInstallDirs)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_BINDIR})

find_package(message 1 CONFIG REQUIRED)
get_property(_loc TARGET message::message-shared PROPERTY LOCATION)
message(STATUS "Found message: ${_loc} (found version ${message_VERSION})")

add_executable(use_message use_message.cpp)

target_link_libraries(use_message
  PUBLIC
    message::message-shared
  )

set_target_properties(use_message
  PROPERTIES
    MACOSX_RPATH ON
    SKIP_BUILD_RPATH OFF
    BUILD_WITH_INSTALL_RPATH OFF
    INSTALL_RPATH "$ORIGIN/../${CMAKE_INSTALL_LIBDIR}"
    INSTALL_RPATH_USE_LINK_PATH ON
  )

enable_testing()

add_test(
  NAME check_use_message
  COMMAND $<TARGET_FILE:use_message>
  )

install(
  PROGRAMS
    ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/use_message
  DESTINATION
    ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_BINDIR}
  )

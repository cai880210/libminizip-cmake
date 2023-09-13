#***************************************************************************
# Copyright (C) 2020 Robbert E. Peters (GizMoCuz)
#***************************************************************************
cmake_minimum_required(VERSION 3.0.2)

project("minizip" C)

# Set cmake debug postfix to d
set(CMAKE_DEBUG_POSTFIX "d")

set(PROJECT_NAME minizip)

INCLUDE(FindZLIB)
include(GNUInstallDirs)
set(INSTALL_BIN_DIR ${CMAKE_INSTALL_BINDIR} CACHE INTERNAL "Installation directory for executables" FORCE)
set(INSTALL_LIB_DIR ${CMAKE_INSTALL_LIBDIR} CACHE INTERNAL "Installation directory for libraries" FORCE)
set(INSTALL_INC_DIR "${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}" CACHE INTERNAL "Installation directory for headers" FORCE)

IF(NOT ZLIB_FOUND)
	message(FATAL_ERROR "zlib library is required for libminizip!")
ENDIF(NOT ZLIB_FOUND)

include_directories(${ZLIB_INCLUDE_DIR})
link_directories(${ZLIB_LIBRARIES})

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

add_library(${PROJECT_NAME} ioapi.c unzip.c zip.c)

target_link_libraries(${PROJECT_NAME} ${ZLIB_LIBRARIES})

install(TARGETS ${PROJECT_NAME}
  RUNTIME DESTINATION ${INSTALL_BIN_DIR}
  LIBRARY DESTINATION ${INSTALL_LIB_DIR}
  ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
)
file(GLOB HEADERS "${PROJECT_SOURCE_DIR}/minizip/*.h")
install(FILES ${HEADERS}
  DESTINATION ${INSTALL_INC_DIR}
)
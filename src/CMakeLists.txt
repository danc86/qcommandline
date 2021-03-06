# Copyright (C) 2009-2011 Corentin Chary <corentin.chary@gmail.com>
#
# This library is free software; you can redistribute it and/or
# modify it under the terms of the GNU Library General Public
# License as published by the Free Software Foundation; either
# version 2 of the License, or (at your option) any later version.
#
# This library is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# Library General Public License for more details.
#
# You should have received a copy of the GNU Library General Public License
# along with this library; see the file COPYING.LIB.  If not, write to
# the Free Software Foundation, Inc., 51 Franklin Street, Fifth Floor,
# Boston, MA 02110-1301, USA.

set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DQT_NO_CAST_FROM_ASCII -DQT_NO_CAST_TO_ASCII" )

include_directories (
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_BINARY_DIR}
  ${QT_INCLUDE_DIRS}
)

install(FILES
  QCommandLine
  qcommandline.h
  DESTINATION ${INCLUDE_INSTALL_DIR}/qcommandline
  COMPONENT devel
)

set(qcommandline_MOC_HDRS qcommandline.h)

qt5_wrap_cpp(qcommandline_MOC_SRCS ${qcommandline_MOC_HDRS})

set (qcommandline_SRCS qcommandline.cpp)

add_library (qcommandline ${qcommandline_SRCS} ${qcommandline_MOC_SRCS})
target_link_libraries( qcommandline Qt5::Core)

if(BUILD_SHARED_LIBS)
  set_target_properties(qcommandline PROPERTIES
    VERSION ${QCOMMANDLINE_LIB_MAJOR_VERSION}.${QCOMMANDLINE_LIB_MINOR_VERSION}.${QCOMMANDLINE_LIB_PATCH_VERSION}
    SOVERSION ${QCOMMANDLINE_LIB_MAJOR_VERSION}
    DEFINE_SYMBOL QCOMMANDLINE_MAKEDLL
  )
else()
	add_definitions(-DQCOMMANDLINE_STATIC)
endif()

install(TARGETS qcommandline
  COMPONENT libraries
  LIBRARY DESTINATION ${LIB_INSTALL_DIR}
  RUNTIME DESTINATION ${BIN_INSTALL_DIR}
  ARCHIVE DESTINATION ${LIB_INSTALL_DIR}
)

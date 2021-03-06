
########################################################
# Files

SET(DB2_SRCS
  qgsdb2geometrycolumns.cpp
  qgsdb2expressioncompiler.cpp
  qgsdb2provider.cpp
  qgsdb2dataitems.cpp
  qgsdb2tablemodel.cpp
  qgsdb2featureiterator.cpp
)
SET(DB2_MOC_HDRS
  qgsdb2provider.h
  qgsdb2dataitems.h
  qgsdb2tablemodel.h
)

IF (WITH_GUI)
  SET(DB2_SRCS ${DB2_SRCS}
    qgsdb2newconnection.cpp
    qgsdb2sourceselect.cpp
  )
  SET(DB2_MOC_HDRS ${DB2_MOC_HDRS}
    qgsdb2newconnection.h
    qgsdb2sourceselect.h
  )
ENDIF ()

########################################################
# Build

QT5_WRAP_CPP(DB2_MOC_SRCS ${DB2_MOC_HDRS})

INCLUDE_DIRECTORIES(
  ${CMAKE_SOURCE_DIR}/src/core
  ${CMAKE_SOURCE_DIR}/src/core/auth
  ${CMAKE_SOURCE_DIR}/src/core/expression
  ${CMAKE_SOURCE_DIR}/src/core/geometry
  ${CMAKE_SOURCE_DIR}/src/core/metadata
  ${CMAKE_SOURCE_DIR}/src/gui
  ${CMAKE_SOURCE_DIR}/src/gui/auth
  ${CMAKE_SOURCE_DIR}/src/ui

  ${CMAKE_BINARY_DIR}/src/core
  ${CMAKE_BINARY_DIR}/src/gui
  ${CMAKE_BINARY_DIR}/src/ui
)

INCLUDE_DIRECTORIES(SYSTEM
  ${GEOS_INCLUDE_DIR}
  ${QT_INCLUDE_DIR}
)

IF (WITH_GUI)
  INCLUDE_DIRECTORIES(SYSTEM
    ${QSCINTILLA_INCLUDE_DIR}
  )
ENDIF ()

ADD_LIBRARY (db2provider MODULE ${DB2_SRCS} ${DB2_HDRS} ${DB2_MOC_SRCS})

TARGET_LINK_LIBRARIES (db2provider
  qgis_core
  ${Qt5Sql_LIBRARIES}
)

IF (WITH_GUI)
  TARGET_LINK_LIBRARIES (db2provider
    qgis_gui
  )
ENDIF ()

########################################################
# Install

INSTALL(TARGETS db2provider
  RUNTIME DESTINATION ${QGIS_PLUGIN_DIR}
  LIBRARY DESTINATION ${QGIS_PLUGIN_DIR})

set(APPNAME kronfit)
set(SOURCES
    kronfit.cpp
    ${SNAPADV_INCLUDE_DIR}/kronecker.h
    ${SNAPADV_SOURCE_DIR}/kronecker.cpp
)

add_snap_example(${APPNAME} SOURCES)

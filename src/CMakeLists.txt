cmake_minimum_required (VERSION 2.6)

find_package (GFAL2 REQUIRED)
find_package (GLIB2 REQUIRED)
find_package (CURL REQUIRED)
find_package (JSONC REQUIRED)

include_directories (${GFAL2_INCLUDE_DIRS})
include_directories (${GLIB2_INCLUDE_DIRS})
include_directories (${CURL_INCLUDE_DIRS})

add_definitions (${GFAL2_DEFINITIONS})
add_definitions (${GLIB2_DEFINITIONS})

add_library (gfal_plugin_sdn SHARED
    gfal_sdn.c
)

target_link_libraries (gfal_plugin_sdn
    ${GFAL2_LIBRARIES}
    ${GLIB2_LIBRARIES}
    ${CURL_LIBRARIES}
)

install (
    TARGETS gfal_plugin_sdn
    LIBRARY DESTINATION ${PLUGIN_INSTALL_DIR}
)

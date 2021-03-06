# This a sample CMakeLists.txt for compiling plug for Switchboard
# http://elementaryos.org/docs/developer-guide/cmake

cmake_minimum_required (VERSION 2.6)
cmake_policy (VERSION 2.6)

# Set the Properties of your Plug

# Set the Name of the plug, should be an executable command, lowercase, no spaces
set (PLUGNAME "locale")
# Set the Category of the Plug, one of:
# personal, hardware, network, system
set (PLUGCATEGORY "personal")
# Set the Title to use for the Plug in the Overview and in the Titlebar
set (PLUGTITLE "Region")
set (PLUGDESCRIPTION "Set region")
# The symbolic name of the Icon to use for the plug
set (PLUGICON "preferences-desktop-locale")
# Set the version for the plug
set (VERSION "0.2.3")

project (${PLUGNAME})

#########################################################
# Only alter stuff below if you know what you are doing #
#########################################################

include (GNUInstallDirs)
set (DATADIR "${CMAKE_INSTALL_FULL_LIBDIR}/switchboard")
set (PKGDATADIR "${DATADIR}/${PLUGCATEGORY}/pantheon-${PLUGNAME}")
set (GETTEXT_PACKAGE "${PLUGNAME}-plug")

list (APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)

# Configure file
#configure_file (plug.plugin ${CMAKE_BINARY_DIR}/${PLUGNAME}.plugin)
#install (FILES ${CMAKE_BINARY_DIR}/${PLUGNAME}.plugin DESTINATION ${PKGDATADIR})
configure_file (${CMAKE_SOURCE_DIR}/config.vala.cmake ${CMAKE_BINARY_DIR}/src/config.vala)
add_definitions (-DGETTEXT_PACKAGE=\"${GETTEXT_PACKAGE}\")

# Comment this out to enable C compiler warnings
add_definitions (-w)

include(GSettings)
add_schema("data/schema/org.pantheon.switchboard.plug.locale.gschema.xml")

# Files
file (GLOB resources "${CMAKE_CURRENT_SOURCE_DIR}/data/*")

#Polkit Policy
configure_file (${CMAKE_SOURCE_DIR}/data/org.pantheon.switchboard.locale.policy.cmake ${CMAKE_BINARY_DIR}/data/org.pantheon.switchboard.locale.policy)
install (FILES ${CMAKE_BINARY_DIR}/data/org.pantheon.switchboard.locale.policy DESTINATION ${CMAKE_INSTALL_FULL_DATAROOTDIR}/polkit-1/actions/)

# Traslation stuff
add_subdirectory (po)

# Info-plug
install (FILES ${CMAKE_CURRENT_SOURCE_DIR}/data/languagelist DESTINATION ${PKGDATADIR})

add_subdirectory(src)

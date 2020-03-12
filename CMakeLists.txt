# CMake build system for proj-data releases.

cmake_minimum_required(VERSION 3.9)

project(PROJ_DATA
    VERSION 1.1
	DESCRIPTION "Data files for PROJ 7 or later"
	LANGUAGES NONE
)
# Only interpret if() arguments as variables or keywords when unquoted
#cmake_policy(SET CMP0054 NEW)

set(CPACK_SOURCE_GENERATOR "TGZ;ZIP")
set(CPACK_BINARY_GENERATOR "NONE")
set(CPACK_INCLUDE_TOPLEVEL_DIRECTORY OFF)
set(CPACK_SOURCE_PACKAGE_FILE_NAME "proj-data-${PROJECT_VERSION}")
set(CPACK_SOURCE_IGNORE_FILES "/travis/;/.github/;/.git/;.swp$;.*~;.py$;.sh$;/scripts/;${CPACK_SOURCE_IGNORE_FILES}")
set(CPACK_INSTALL_COMMANDS
    "${CMAKE_COMMAND} -E make_directory ${CMAKE_BINARY_DIR}/tmp"
    "${CMAKE_COMMAND} -E copy ${CMAKE_SOURCE_DIR}/README.DATA ${CMAKE_BINARY_DIR}/tmp"
    "${CMAKE_COMMAND} -E copy ${CMAKE_SOURCE_DIR}/copyright_and_licenses.csv ${CMAKE_BINARY_DIR}/tmp"
)

macro(install_directory SRCDIR)
  message(STATUS "from ${CMAKE_SOURCE_DIR}/${SRCDIR} to ${CMAKE_BINARY_DIR}")
  install(DIRECTORY "${CMAKE_SOURCE_DIR}/${SRCDIR}" DESTINATION "${CMAKE_BINARY_DIR}")
endmacro()

install_directory("at_bev/")
set(CPACK_SOURCE_INSTALLED_DIRECTORIES
    "${CMAKE_BINARY_DIR}/tmp/" "."
    # "${CMAKE_CURRENT_SOURCE_DIR}/at_bev/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/au_ga/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/au_icsm/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/be_ign/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/ca_nrc/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/ca_que_mern/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/ch_swisstopo/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/de_adv/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/de_geosn/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/de_lgl_bw/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/de_lgvl_saarland/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/dk_sdfe/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/es_cat_icgc/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/eur_nkg/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/fr_ign/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/is_lmi/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/nc_dittt/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/nl_nsgi/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/nz_linz/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/pt_dgt/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/se_lantmateriet/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/uk_os/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/us_nga/" "."
    "${CMAKE_CURRENT_SOURCE_DIR}/us_noaa/" "."
)

include(CPack)

add_custom_target(dist COMMAND ${CMAKE_MAKE_PROGRAM} package_source)

FIND_PACKAGE(SWIG REQUIRED)
INCLUDE(${SWIG_USE_FILE})
FIND_PACKAGE(Ruby REQUIRED)
INCLUDE_DIRECTORIES(${RUBY_INCLUDE_DIRS})
include_directories(${PROJECT_SOURCE_DIR}/mapscript/swiginc)
include_directories(${PROJECT_SOURCE_DIR}/mapscript/)
include_directories(${PROJECT_SOURCE_DIR}/mapscript/ruby)
SWIG_ADD_MODULE(rubymapscript ruby ../mapscript.i)

SWIG_LINK_LIBRARIES(rubymapscript ${RUBY_LIBRARY} ${MAPSERVER_LIBMAPSERVER})

set_target_properties(${SWIG_MODULE_rubymapscript_REAL_NAME} PROPERTIES PREFIX "")
set_target_properties(${SWIG_MODULE_rubymapscript_REAL_NAME} PROPERTIES OUTPUT_NAME mapscript)

get_target_property(LOC_MAPSCRIPT_LIB ${SWIG_MODULE_rubymapscript_REAL_NAME} LOCATION)
execute_process(COMMAND ${RUBY_EXECUTABLE} -r rbconfig -e "puts RbConfig::CONFIG['archdir']" OUTPUT_VARIABLE RUBY_ARCHDIR OUTPUT_STRIP_TRAILING_WHITESPACE)
install(FILES ${LOC_MAPSCRIPT_LIB} DESTINATION ${RUBY_ARCHDIR})

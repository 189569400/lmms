INCLUDE_DIRECTORIES("${CMAKE_SOURCE_DIR}/include")
FILE(GLOB SOURCES *.cc)
LIST(SORT SOURCES)
ADD_LIBRARY(caps MODULE ${SOURCES})
INSTALL(TARGETS caps LIBRARY DESTINATION "${PLUGIN_DIR}/ladspa")
IF(LMMS_BUILD_WIN64)
ADD_DEFINITIONS(-DLMMS_BUILD_WIN64)
ENDIF(LMMS_BUILD_WIN64)
SET_TARGET_PROPERTIES(caps PROPERTIES PREFIX "")
SET_TARGET_PROPERTIES(caps PROPERTIES COMPILE_FLAGS "-O2 -funroll-loops -Wno-write-strings")

IF(LMMS_BUILD_WIN32)
	add_custom_command(
		TARGET caps
		POST_BUILD
		COMMAND "${STRIP_COMMAND}" "$<TARGET_FILE:caps>"
		VERBATIM
		COMMAND_EXPAND_LISTS
	)
ENDIF(LMMS_BUILD_WIN32)
IF(NOT LMMS_BUILD_APPLE AND NOT LMMS_BUILD_OPENBSD)
	SET_TARGET_PROPERTIES(caps PROPERTIES LINK_FLAGS "${LINK_FLAGS} -shared -Wl,-no-undefined")
ENDIF(NOT LMMS_BUILD_APPLE AND NOT LMMS_BUILD_OPENBSD)

IF(LMMS_BUILD_LINUX)
	SET_TARGET_PROPERTIES(caps PROPERTIES LINK_FLAGS "${LINK_FLAGS}")
ENDIF(LMMS_BUILD_LINUX)


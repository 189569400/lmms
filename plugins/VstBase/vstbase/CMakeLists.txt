ADD_DEFINITIONS(-DREMOTE_VST_PLUGIN_FILEPATH_32="${REMOTE_VST_PLUGIN_FILEPATH_32}")
ADD_DEFINITIONS(-DREMOTE_VST_PLUGIN_FILEPATH_64="${REMOTE_VST_PLUGIN_FILEPATH_64}")
IF(LMMS_BUILD_LINUX)
	ADD_DEFINITIONS(-DNATIVE_LINUX_REMOTE_VST_PLUGIN_FILEPATH_64="${NATIVE_LINUX_REMOTE_VST_PLUGIN_FILEPATH_64}")
ENDIF()

BUILD_PLUGIN(vstbase
	../vst_base.cpp ../VstPlugin.cpp ../VstPlugin.h ../communication.h
	MOCFILES ../VstPlugin.h
	EXPORT_BASE_NAME vstbase
	LINK SHARED
)

TARGET_INCLUDE_DIRECTORIES(vstbase
	PUBLIC ../
)

IF(LMMS_BUILD_LINUX)
	TARGET_LINK_LIBRARIES(vstbase qx11embedcontainer)
ENDIF()

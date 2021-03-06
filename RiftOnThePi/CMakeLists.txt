CMAKE_MINIMUM_REQUIRED( VERSION 2.8 )

PROJECT( "RiftOnThePi" )

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/" )
find_package(OVR)
	
SET(	SOURCES
		Common.h
		Common.cpp
		RiftOnThePiApp.h
		RiftOnThePiApp.cpp
		Main.cpp 
	)

SOURCE_GROUP("" FILES ${SOURCES} )		# Avoid "Header Files" and "Source Files" virtual folders in VisualStudio

IF( CMAKE_SYSTEM_NAME MATCHES "Windows" )

	IF( MSVC )
		INCLUDE( VisualStudioSettings.cmake )
	ENDIF()

	SET( EXTRA_INCLUDE_DIRS ${EXTRA_INCLUDE_DIRS}
		 "${CMAKE_CURRENT_LIST_DIR}/../Dependencies/gles_sdk/include"
		 ${OpenGLESSandboxLib_SOURCE_DIR} )

ELSEIF( CMAKE_SYSTEM_NAME MATCHES "Linux" )
	
	# CFLAGS+=-DSTANDALONE -D__STDC_CONSTANT_MACROS -D__STDC_LIMIT_MACROS -DTARGET_POSIX -D_LINUX -fPIC -DPIC -D_REENTRANT -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64 -U_FORTIFY_SOURCE -Wall -g -DHAVE_LIBOPENMAX=2 -DOMX -DOMX_SKIP64BIT -ftree-vectorize -pipe -DUSE_EXTERNAL_OMX -DHAVE_LIBBCM_HOST -DUSE_EXTERNAL_LIBBCM_HOST -DUSE_VCHIQ_ARM -Wno-psabi
	ADD_DEFINITIONS( 
		-DSTANDALONE 
		-D__STDC_CONSTANT_MACROS 
		-D__STDC_LIMIT_MACROS 
		-DTARGET_POSIX 
		-D_LINUX 
		-fPIC 
		-DPIC 
		-D_REENTRANT 
		-D_LARGEFILE64_SOURCE 
		-D_FILE_OFFSET_BITS=64 
		-U_FORTIFY_SOURCE 
		-Wall 
		-g 
		-DHAVE_LIBOPENMAX=2 
		-DOMX 
		-DOMX_SKIP64BIT 
		-ftree-vectorize 
		-pipe 
		-DUSE_EXTERNAL_OMX 
		-DHAVE_LIBBCM_HOST 
		-DUSE_EXTERNAL_LIBBCM_HOST 
		-DUSE_VCHIQ_ARM 
		-Wno-psabi )

	SET( EXTRA_INCLUDE_DIRS ${EXTRA_INCLUDE_DIRS}
		"/opt/vc/include"			#  bcm_host.h
		"/opt/vc/include/interface/vcos/pthreads"
		"/opt/vc/include/interface/vmcs_host/linux" )
	
	SET( EXTRA_LINK_DIRS ${EXTRA_LINK_DIRS}
		"/opt/vc/lib" )
ENDIF()
	
INCLUDE_DIRECTORIES( ${OpenGLESSandboxLib_SOURCE_DIR} )
INCLUDE_DIRECTORIES( ${EXTRA_INCLUDE_DIRS} )
INCLUDE_DIRECTORIES( ${LibOVR_SOURCE_DIR}/Include )
INCLUDE_DIRECTORIES( ${OVR_INCLUDE_DIR} )
INCLUDE_DIRECTORIES( ${OVR_SOURCE_DIR} )
INCLUDE_DIRECTORIES( ${_ovr_dependency_includes} )

LINK_DIRECTORIES( ${EXTRA_LINK_DIRS} )

ADD_EXECUTABLE( ${PROJECT_NAME} ${SOURCES} )

TARGET_LINK_LIBRARIES( ${PROJECT_NAME} 
						OpenGLESSandboxLib 
						${EXTRA_LIBS} ${OVR_LIBRARIES} )
						 
	

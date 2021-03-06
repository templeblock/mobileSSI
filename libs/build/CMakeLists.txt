    #-----------------------------------------------------------------------------
# Initialize project.

if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
	if(NOT DEFINED WITH_IN_SOURCE_BUILD)
		message(FATAL_ERROR
			"CMake generation for openssi is not allowed within the source directory!"
			"/n Remove the CMakeCache.txt file and try again from another folder, e.g.:"
			"/n "
			"/n rm CMakeCache.txt"
			"/n cd .."
			"/n mkdir cmake-make"
			"/n cd cmake-make"
			"/n cmake ../core"
			"/n "
			"/n Alternately define WITH_IN_SOURCE_BUILD to force this option (not recommended!)"
		)		
	endif()
endif()

cmake_minimum_required(VERSION 2.8)

project(openssi-libs)

include_directories()

add_subdirectory(ExpressionParser)
add_subdirectory(kissfft)
add_dependencies(kissFFT ssi)
add_subdirectory(AlgLib)
add_subdirectory(ssiml)
#add_subdirectory(MlpXml)
add_subdirectory(emovoice)
add_subdirectory(liblinear-2.1)
#add_subdirectory(liblinear-incdec-2.01)
#add_subdirectory(tensorflow/tensorflow/contrib/cmake)
#add_subdirectory(gloox)

add_custom_command(TARGET ssiml
        PRE_BUILD
        COMMAND /bin/sh ${CMAKE_SOURCE_DIR}/mobileSSI/libs/build/OpenSMILE-2.1.0/buildWithInstalledPortAudio.sh
        )


if(ANDROID)
add_subdirectory(libzip)
else(ANDROID)
add_subdirectory(serial)
endif(ANDROID)

#install(TARGETS openssi-core DESTINATION bin)

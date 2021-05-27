cmake_minimum_required(VERSION 3.10)
project(pocktool)

include(FindZIP.cmake)

ZIP_UNPACK_TARGET(
		pocket-tools-unzip
		${CMAKE_CURRENT_BINARY_DIR}
		${CMAKE_CURRENT_SOURCE_DIR}/pocket-tools.zip
		POCKTOOL/Sources/bas2img_600.c
		POCKTOOL/Sources/bin2wav_200b.c
		POCKTOOL/Sources/wav2bin_200a.c
	)

add_executable(
	bas2img
	POCKTOOL/Sources/bas2img_600.c
	)

add_executable(
	bin2wav
	POCKTOOL/Sources/bin2wav_200b.c
	)

add_executable(
	wav2bin
	POCKTOOL/Sources/wav2bin_200a.c
	)

function(bas2img)
	set(options)

	set(oneValueArgs
		BAS_FILE
		IMG_FILE
		PC_TYPE
	)
	set(multiValueArgs
	)
	cmake_parse_arguments(B2I "${options}" "${oneValueArgs}" "${multiValueArgs}" ${ARGN})
	
	add_custom_command(
		OUTPUT
			${B2I_IMG_FILE}
		COMMAND
		bas2img
		-p ${B2I_PC_TYPE}
		${B2I_BAS_FILE}
		${B2I_IMG_FILE}
		DEPENDS
			bas2img
			${B2I_BAS_FILE}
		)

	add_custom_target(${B2I_IMG_FILE}-tgt ALL DEPENDS ${B2I_IMG_FILE})

endfunction()

function(img2wav)
	set(options)

	set(oneValueArgs
		IMG_FILE
		WAV_FILE
		PC_TYPE
		NAME
		ADDR
		TYPE
	)
	set(multiValueArgs
	)
	cmake_parse_arguments(B2W "${options}" "${oneValueArgs}" "${multiValueArgs}" ${ARGN})
	
	set(startaddr)
	if(NOT "${B2W_ADDR}" STREQUAL "")
		set(startaddr -a ${B2W_ADDR})
	endif()

	add_custom_command(
		OUTPUT
			${B2W_WAV_FILE}
		COMMAND
			bin2wav
			-p ${B2W_PC_TYPE}
			-t ${B2W_TYPE}
			--name=${B2W_NAME}
			${startaddr}
			${B2W_IMG_FILE}
			${B2W_WAV_FILE}
		DEPENDS
			bin2wav
			${B2W_IMG_FILE}
		)

	add_custom_target(${B2W_WAV_FILE}-tgt ALL DEPENDS ${B2W_WAV_FILE})

endfunction()

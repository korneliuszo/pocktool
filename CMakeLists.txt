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

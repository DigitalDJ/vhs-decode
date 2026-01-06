pkg_check_modules(FFMPEG REQUIRED IMPORTED_TARGET
    libavcodec
    libavformat
    libavutil
)

add_executable(ld-ldf-reader
    ldfreader.cpp
    main.cpp
)

target_link_libraries(ld-ldf-reader PRIVATE Qt::Core PkgConfig::FFMPEG lddecode-library)

if(VCPKG_TARGET_TRIPLET STREQUAL "x64-windows-static-md" OR VCPKG_TARGET_TRIPLET STREQUAL "x64-windows-static")
# Needed on msvc with vcpkg - may need equivialent on mingw
# see https://github.com/CCExtractor/rusty_ffmpeg/pull/37
	message(STATUS "windows static build")
	target_link_libraries(ld-ldf-reader wsock32 ws2_32 Secur32 Strmiids BCrypt Mfuuid)
endif()

install(TARGETS ld-ldf-reader)

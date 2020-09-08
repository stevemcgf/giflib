cmake_minimum_required(VERSION 3.13)
cmake_policy(VERSION 3.13)

project(giflib C)

set(INSTALL_BIN_DIR "${CMAKE_INSTALL_PREFIX}/bin" CACHE PATH "Installation directory for executables")
set(INSTALL_LIB_DIR "${CMAKE_INSTALL_PREFIX}/lib" CACHE PATH "Installation directory for libraries")
set(INSTALL_INC_DIR "${CMAKE_INSTALL_PREFIX}/include" CACHE PATH "Installation directory for headers")

set(GIFLIB_HEADERS
    include/gif_lib.h
)

set(GIFLIB_SOURCES
    dgif_lib.c
    egif_lib.c
    gif_err.c
    gif_font.c
    gif_hash.c
    gifalloc.c
    openbsd-reallocarray.c
    quantize.c
)

add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_library(gif STATIC ${GIFLIB_SOURCES} ${GIFLIB_HEADERS})
target_include_directories(gif PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
add_library(giflib::gif ALIAS gif)

if(NOT SKIP_INSTALL_LIBRARIES AND NOT SKIP_INSTALL_ALL )
    install(TARGETS gif
        RUNTIME DESTINATION "${INSTALL_BIN_DIR}"
        LIBRARY DESTINATION "${INSTALL_LIB_DIR}"
        ARCHIVE DESTINATION "${INSTALL_LIB_DIR}"
    )
endif()

if(NOT SKIP_INSTALL_HEADERS AND NOT SKIP_INSTALL_ALL )
    install(FILES ${GIFLIB_HEADERS} DESTINATION "${INSTALL_INC_DIR}")
endif()


cmake_minimum_required(VERSION 3.1)
project(game.libretro.beetle-pcfx)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

find_package(Kodi REQUIRED)

include(ExternalProject)

if(CMAKE_BUILD_TYPE MATCHES Debug)
  set(LIBRETRO_DEBUG DEBUG=1)
endif()

if("${CORE_SYSTEM_NAME}" STREQUAL "windows")
  set(BUILD_COMMAND mingw32-make platform=win ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "linux")
  set(BUILD_COMMAND make ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "darwin")
  set(BUILD_COMMAND make platform=osx ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "ios")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "android")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "rbpi")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "freebsd")
# TODO
endif()

externalproject_add(beetle-pcfx
                    GIT_REPOSITORY https://github.com/libretro/beetle-pcfx-libretro
                    GIT_TAG ac6387d
                    PREFIX beetle-pcfx
                    CONFIGURE_COMMAND ""
                    INSTALL_COMMAND ""
                    BUILD_COMMAND ${BUILD_COMMAND}
                    BUILD_IN_SOURCE 1)

set(BEETLEPCFX_CUSTOM_BINARY ${PROJECT_BINARY_DIR}/beetle-pcfx/src/beetle-pcfx/mednafen_pcfx_libretro${CMAKE_SHARED_LIBRARY_SUFFIX}
                             ${PROJECT_NAME}${CMAKE_SHARED_LIBRARY_SUFFIX})

build_addon(${PROJECT_NAME} BEETLEPCFX DEPLIBS)
add_dependencies(${PROJECT_NAME} beetle-pcfx)

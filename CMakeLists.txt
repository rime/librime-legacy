project(rime-legacy)
cmake_minimum_required(VERSION 2.8)

set(KyotoCabinet_STATIC ${BUILD_STATIC})

find_package(ZLIB)
find_package(KyotoCabinet REQUIRED)
if(KyotoCabinet_FOUND)
  include_directories(${KyotoCabinet_INCLUDE_PATH})
endif(KyotoCabinet_FOUND)

aux_source_directory(src rime_legacy_src)

add_library(rime-legacy-objs OBJECT ${rime_legacy_src})
if(BUILD_SHARED_LIBS)
  set_target_properties(rime-legacy-objs
    PROPERTIES
    POSITION_INDEPENDENT_CODE ON)
endif()

set(plugin_name "rime-legacy" PARENT_SCOPE)
set(plugin_objs $<TARGET_OBJECTS:rime-legacy-objs> PARENT_SCOPE)
set(plugin_deps
  ${KyotoCabinet_LIBRARY}
  ${ZLIB_LIBRARY}
  ${rime_library}
  ${rime_dict_library}
  PARENT_SCOPE)
# never used as a loaded-by-default module, but explicitly loaded
# in a deployment task for one-off user dictionary upgrade.
# thus comment out this line.
# set(plugin_modules "legacy" PARENT_SCOPE)

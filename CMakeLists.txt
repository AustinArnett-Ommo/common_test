include_guard()

set(JSON_FC_TARGET fc_json)
set(JSON_EXT_TARGET ext_json)
set(_JSON_REPO "https://github.com/nlohmann/json.git")
set(_JSON_VERSION "3.10.5")
set(_JSON_TAG "v${_JSON_VERSION}")
set(_JSON_LIB_NAME "nlohmann_json")
set(_JSON_CONFIG_DIR "lib/cmake/nlohmann_json")
set(_DEFAULT_REQUIRED_CONFIG "Release")

FetchContent_Declare(${JSON_FC_TARGET}
  GIT_REPOSITORY ${_JSON_REPO}
  GIT_TAG ${_JSON_TAG})

FetchContent_GetProperties(${JSON_FC_TARGET})
    if (NOT ${JSON_FC_TARGET}_POPULATED)
        FetchContent_Populate(${JSON_FC_TARGET})
        add_subdirectory(${${JSON_FC_TARGET}_SOURCE_DIR} ${${JSON_FC_TARGET}_BINARY_DIR} EXCLUDE_FROM_ALL)
    endif()

include(ExternalProject)
ExternalProject_Add(${JSON_EXT_TARGET}
  PREFIX "${DEPS_BUILD_DIR}/${_JSON_LIB_NAME}_build"
  SOURCE_DIR "${${JSON_FC_TARGET}_SOURCE_DIR}"
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX:PATH=install/${OMMO_BUILD_TYPE}/${_JSON_LIB_NAME}
    -DCMAKE_BUILD_TYPE:STRING=${CMAKE_BUILD_TYPE}
    )

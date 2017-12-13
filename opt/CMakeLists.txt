macro(set_cache_variable VAR_NAME VAR_DESCRIPTION)
    set(${VAR_NAME} ${${VAR_NAME}} CACHE INTERNAL ${VAR_DESCRIPTION})
    message(STATUS "Set ${VAR_NAME} to ${${VAR_NAME}}.")
endmacro()


# Curl configuration

if(USE_SYSTEM_CURL)
    find_package(CURL)
endif()
if(NOT USE_SYSTEM_CURL OR NOT CURL_FOUND)
    message(STATUS "Not using system Curl, using built-in curl project instead.")
    option(BUILD_TESTING "Set to ON to build cURL tests." OFF)
    option(BUILD_CURL_EXE "Set to ON to build cURL executable." OFF)
    add_subdirectory(curl)
    set(CURL_FOUND TRUE)
    set(CURL_LIBRARIES libcurl)
    set(CURL_INCLUDE_DIRS
        ${CURL_SOURCE_DIR}/include
        ${CURL_BINARY_DIR}/include/curl)

    # Group under the "external" project folder in IDEs such as Visual Studio.
    if(BUILD_CURL_EXE)
        set_property(TARGET curl PROPERTY FOLDER "external")
    endif()

    set_property(TARGET libcurl PROPERTY FOLDER "external")
endif()

set_cache_variable(CURL_FOUND "Set if libcurl is found or built")
set_cache_variable(CURL_LIBRARIES "Location of libcurl")
set_cache_variable(CURL_INCLUDE_DIRS "Location of curl include files")


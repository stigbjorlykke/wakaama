cmake_minimum_required (VERSION 2.8)

project (lwm2mserver C)

if(DTLS)
    message(FATAL_ERROR "DTLS option is not supported." )
endif()

include(${CMAKE_CURRENT_LIST_DIR}/../../core/wakaama.cmake)
include(${CMAKE_CURRENT_LIST_DIR}/../../coap/coap.cmake)
include(${CMAKE_CURRENT_LIST_DIR}/../../data/data.cmake)
include(${CMAKE_CURRENT_LIST_DIR}/../shared/shared.cmake)

add_definitions(-DLWM2M_SERVER_MODE)
add_definitions(${SHARED_DEFINITIONS} ${WAKAAMA_DEFINITIONS})

include_directories (${WAKAAMA_HEADERS_DIR} ${COAP_HEADERS_DIR} ${DATA_HEADERS_DIR} ${WAKAAMA_SOURCES_DIR} ${SHARED_INCLUDE_DIRS})

SET(SOURCES
    ${CMAKE_CURRENT_LIST_DIR}/lwm2mserver.c
    )

add_executable(${PROJECT_NAME} ${SOURCES} ${WAKAAMA_SOURCES} ${COAP_SOURCES} ${DATA_SOURCES} ${SHARED_SOURCES})

# Add WITH_LOGS to debug variant
set_property(TARGET ${PROJECT_NAME} APPEND PROPERTY COMPILE_DEFINITIONS $<$<CONFIG:Debug>:WITH_LOGS>)

SOURCE_GROUP(wakaama FILES ${WAKAAMA_SOURCES})

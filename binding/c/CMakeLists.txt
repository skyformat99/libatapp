﻿set(PROJECT_BINDING_NAME "atapp_c")
EchoWithColor(COLOR GREEN "-- Configure ${CMAKE_CURRENT_LIST_DIR}")

# ============ c binding ============

file(GLOB BINDING_SRC_LIST RELATIVE "${CMAKE_CURRENT_LIST_DIR}"
    ${CMAKE_CURRENT_LIST_DIR}/atframe/*.cpp
    ${CMAKE_CURRENT_LIST_DIR}/atframe/*.cc
    ${CMAKE_CURRENT_LIST_DIR}/atframe/*.c
    ${CMAKE_CURRENT_LIST_DIR}/atframe/*.cxx
)
include_directories(${CMAKE_CURRENT_LIST_DIR})

if ( NOT MSVC AND NOT APPLE )
    add_definitions(-shared)
endif()

# ============ libatbus - src ============
add_library(${PROJECT_BINDING_NAME} SHARED ${BINDING_SRC_LIST} ${PROJECT_3RD_PARTY_SRC_LIST})

target_link_libraries(${PROJECT_BINDING_NAME}
    ${PROJECT_LIB_LINK}
    atapp
    ${3RD_PARTY_LIBATBUS_LINK_NAME}
    ${3RD_PARTY_ATFRAME_UTILS_LINK_NAME}
    ${3RD_PARTY_LIBUV_LINK_NAME}
    ${COMPILER_OPTION_EXTERN_CXX_LIBS}
)

install(TARGETS ${PROJECT_BINDING_NAME}
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib${PLATFORM_SUFFIX}
    ARCHIVE DESTINATION lib${PLATFORM_SUFFIX}
)

install(FILES ${CMAKE_CURRENT_LIST_DIR}/atframe/libatapp_c.h
    DESTINATION include/atframe
)
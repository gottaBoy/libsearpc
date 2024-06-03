# lib/CMakeLists.txt
set(SOURCE_FILES
    searpc-client.c 
    searpc-server.c 
    searpc-utils.c 
    searpc-named-pipe-transport.c
)

add_library(searpc SHARED ${SOURCE_FILES})

target_link_libraries(searpc
    ${GLIB_LIBRARIES}
    ${JANSSON_LIBRARIES}
    ${LIB_WS32}
)

install(TARGETS searpc
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/ DESTINATION include)

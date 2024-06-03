# demo/CMakeLists.txt

set(GENERATED_SOURCES
    ${CMAKE_CURRENT_BINARY_DIR}/searpc-signature.h
    ${CMAKE_CURRENT_BINARY_DIR}/searpc-marshal.h
)

add_custom_command(
    OUTPUT ${GENERATED_SOURCES}
    COMMAND ${PYTHON_EXECUTABLE} ${PROJECT_SOURCE_DIR}/lib/searpc-codegen.py ${PROJECT_SOURCE_DIR}/demo/rpc_table.py
    DEPENDS ${PROJECT_SOURCE_DIR}/demo/rpc_table.py ${PROJECT_SOURCE_DIR}/lib/searpc-codegen.py
    COMMENT "Generating RPC header files"
)

add_custom_target(gensource DEPENDS ${GENERATED_SOURCES})

include_directories(${CMAKE_CURRENT_BINARY_DIR})

set(searpc_demo_server_SOURCES
    test-object.c
    searpc-demo-server.c
    searpc-demo-packet.h
    ${GENERATED_SOURCES}
)

set(searpc_demo_client_SOURCES
    test-object.c
    searpc-demo-client.c
    searpc-demo-packet.h
    ${GENERATED_SOURCES}
)

set(searpc_async_client_SOURCES
    demo-async-client.c
    searpc-demo-packet.h
    ${GENERATED_SOURCES}
)

add_executable(searpc-demo-server ${searpc_demo_server_SOURCES})
add_executable(searpc-demo-client ${searpc_demo_client_SOURCES})
add_executable(searpc-async-client ${searpc_async_client_SOURCES})

target_link_libraries(searpc-demo-server searpc ${GLIB_LIBRARIES} ${JANSSON_LIBRARIES} ${LIB_WS32})
target_link_libraries(searpc-demo-client searpc ${GLIB_LIBRARIES} ${JANSSON_LIBRARIES} ${LIB_WS32})
target_link_libraries(searpc-async-client searpc ${GLIB_LIBRARIES} ${JANSSON_LIBRARIES} ${LIB_WS32})

add_dependencies(searpc-demo-server gensource)
add_dependencies(searpc-demo-client gensource)
add_dependencies(searpc-async-client gensource)

install(TARGETS searpc-demo-server searpc-demo-client searpc-async-client RUNTIME DESTINATION bin)

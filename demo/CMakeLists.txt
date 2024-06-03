# demo/CMakeLists.txt
set(DEMO_SOURCE_FILES
    searpc-demo-client.c
    searpc-demo-server.c
)

foreach(file ${DEMO_SOURCE_FILES})
    get_filename_component(exe ${file} NAME_WE)
    add_executable(${exe} ${file})
    target_link_libraries(${exe} searpc ${GLIB_LIBRARIES} ${JANSSON_LIBRARIES} ${LIB_WS32})
    install(TARGETS ${exe} RUNTIME DESTINATION bin)
endforeach()

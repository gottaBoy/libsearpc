# tests/CMakeLists.txt

include_directories(${CMAKE_BINARY_DIR}/demo) # Include generated headers
set(clar_suite_sources ${CMAKE_BINARY_DIR}/clar.suite)
# Custom command to generate clar.suite
add_custom_command(
    OUTPUT ${clar_suite_sources}
    COMMAND ${PYTHON_EXECUTABLE} ${CMAKE_SOURCE_DIR}/tests/generate.py
    DEPENDS ${CMAKE_SOURCE_DIR}/tests/generate.py
    COMMENT "Generating clar.suite"
)
# Custom target to group the generated sources
add_custom_target(gensource ALL DEPENDS ${clar_suite_sources})


set(TEST_SOURCE_FILES
    searpc.c 
    clar.c 
    main.c
    gensource
)

add_executable(searpc-test ${TEST_SOURCE_FILES})
target_link_libraries(searpc-test searpc ${GLIB_LIBRARIES} ${JANSSON_LIBRARIES} ${LIB_WS32})

# Add cleaning rules
# set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "${generated_sources};rpc_table.pyc;rpc_table.stamp;rpc_table.tmp;generate.pyc")

install(TARGETS searpc-test RUNTIME DESTINATION bin)

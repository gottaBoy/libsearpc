# tests/CMakeLists.txt
set(TEST_SOURCE_FILES
    searpc-test.c
)

add_executable(searpc-test ${TEST_SOURCE_FILES})
target_link_libraries(searpc-test searpc ${GLIB_LIBRARIES} ${JANSSON_LIBRARIES} ${LIB_WS32})
install(TARGETS searpc-test RUNTIME DESTINATION bin)

set(TESTS_INTEGRATION
    MALLOCX_ARENA.c
    aligned_alloc.c
    allocated.c
    # extent.c # not a valid test
    mallocx.c
    overflow.c
    posix_memalign.c
    rallocx.c
    sdallocx.c
    thread_arena.c
    thread_tcache_enabled.c
    xallocx.c
    )

##########################################################################
# Build integration tests
# This links to SO library and UTIL_INTEGRATION
foreach(sourcefile ${TESTS_INTEGRATION})
    get_filename_component(exename ${sourcefile} NAME_WE)
    set(exename "${exename}${install_suffix}")
    add_executable(${exename} ${sourcefile})
    add_dependencies(${exename} ${PROJECT_NAME}-jet-static)

    target_include_directories(${exename} PRIVATE ${PROJECT_SOURCE_DIR}/test/include)
    target_compile_definitions(${exename} PRIVATE JEMALLOC_INTEGRATION_TEST)

    target_link_libraries(${exename} ${C_TESTLIB_INTEGRATION} ${PROJECT_NAME}-static)
    target_link_libraries(${exename} pthread)

    add_test(NAME ${exename} COMMAND ${exename})
    add_dependencies(check-jemalloc ${exename})
endforeach(sourcefile ${TESTS_UNIT})

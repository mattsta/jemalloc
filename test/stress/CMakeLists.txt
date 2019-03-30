############################################################################
# Microbench
set(TESTS_STRESS
    microbench.c
    )

# Build benchmark
# This consumes C_JETLIB, Link to SO
foreach(sourcefile ${TESTS_STRESS})
    get_filename_component(exename ${sourcefile} NAME_WE)
    set(exename "${exename}${install_suffix}")
    add_executable(${exename} ${sourcefile})
    add_dependencies(${exename} ${PROJECT_NAME}-jet-static)

    target_include_directories(${exename} PRIVATE
        ${PROJECT_SOURCE_DIR}/test/include)
    target_compile_definitions(${exename} PRIVATE JEMALLOC_STRESS_TEST)

    # We can't use {static, static} here because of name conflicts
    target_link_libraries(${exename} ${C_TESTLIB_STRESS} ${PROJECT_NAME}-shared)
    target_link_libraries(${exename} ${PROJECT_NAME}-jet-static)
    target_link_libraries(${exename} pthread)
endforeach(sourcefile ${TESTS_UNIT})

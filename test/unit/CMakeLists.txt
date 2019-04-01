###########################################################################
# Build Unit tests
# This links to exported C_JETLIB static version of the library

set(UnitTests
    SFMT.c
    a0.c
    arena_reset.c
    arena_reset_prof.c
    atomic.c
    background_thread.c
    background_thread_enable.c
    base.c
    binshard.c
    bit_util.c
    bitmap.c
    ckh.c
    decay.c
    div.c
    emitter.c
    extent_quantize.c
    fork.c
    hash.c
    hook.c
    huge.c
    junk.c
    junk_alloc.c
    junk_free.c
    log.c
    mallctl.c
    malloc_io.c
    math.c
    mq.c
    mtx.c
    nstime.c
    pack.c
    pages.c
    ph.c
    prng.c
    prof_accum.c
    prof_active.c
    prof_gdump.c
    prof_idump.c
    prof_log.c
    prof_reset.c
    prof_tctx.c
    prof_thread_name.c
    ql.c
    qr.c
    rb.c
    retained.c
    rtree.c
    sc.c
    seq.c
    size_classes.c
    slab.c
    smoothstep.c
    spin.c
    stats.c
    stats_print.c
    test_hooks.c
    ticker.c
    tsd.c
    witness.c
    zero.c
    )

set(C_UNIT_TEST_C_FLAGS ${COMMON_TESTLIB_CC_FLAGS})
if(MSVC)
    # Turn off warnings
    # test\unit\tsd.c: 4312: bitmap signed/unsigned
    set(C_UNIT_TEST_C_FLAGS "${C_UNIT_TEST_C_FLAGS};/wd4312")
endif()

foreach(sourcefile ${UnitTests})
    get_filename_component(exename ${sourcefile} NAME_WE)
    set(exename "${exename}${install_suffix}")
    add_executable(${exename} ${sourcefile})
    add_dependencies(${exename} ${PROJECT_NAME}-jet-static)
    target_include_directories(${exename} PRIVATE
        ${PROJECT_SOURCE_DIR}/test/include)

    target_compile_options(${exename} PRIVATE "${C_UNIT_TEST_C_FLAGS}")

    set_target_properties(${exename} PROPERTIES
        COMPILE_DEFINITIONS
        "${C_UNITETEST_DEFS}")
    target_link_libraries(${exename} ${PROJECT_NAME}UnitTest)
    target_link_libraries(${exename} ${PROJECT_NAME}-jet-static)
    target_link_libraries(${exename} pthread dl)

    add_test(NAME ${exename} COMMAND ${exename})
    add_dependencies(check-jemalloc ${exename})
endforeach(sourcefile ${TESTS_UNIT})

target_link_libraries(math m)

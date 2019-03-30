############################################################################
# Common source for Unit, Integration and stress test libraries
set(UnitTestCommon
    SFMT.c
    btalloc.c
    btalloc_0.c
    btalloc_1.c
    math.c
    mq.c
    mtx.c
    test.c
    thd.c
    timer.c
    )

############################################################################
# Unit tests test library
# Unit test have access to all of the interfaces
# and we link to a JET library thus JEMALLOC_JET is automatically defined
add_library(${PROJECT_NAME}UnitTest STATIC ${UnitTestCommon})
add_dependencies(${PROJECT_NAME}UnitTest ${PROJECT_NAME}-jet-static)
target_include_directories(${PROJECT_NAME}UnitTest PRIVATE
    ${PROJECT_SOURCE_DIR}/test/include)
target_compile_options(${PROJECT_NAME}UnitTest PRIVATE
    "${COMMON_TESTLIB_CC_FLAGS}")
target_compile_definitions(${PROJECT_NAME}UnitTest PRIVATE
    "${C_TESTLIB_UNIT_DEFS}")

############################################################################
# Integration test library
add_library(${C_TESTLIB_INTEGRATION} STATIC ${UnitTestCommon})
add_dependencies(${C_TESTLIB_INTEGRATION} ${PROJECT_NAME}-jet-static)
target_include_directories(${C_TESTLIB_INTEGRATION} PRIVATE
    ${PROJECT_SOURCE_DIR}/test/include)
target_compile_options(${C_TESTLIB_INTEGRATION} PRIVATE
    "${COMMON_TESTLIB_CC_FLAGS}")
target_compile_definitions(${C_TESTLIB_INTEGRATION} PRIVATE
    "${C_TESTLIB_INTEGRATION_DEFS}")

############################################################################
# Stress test library to link with microbench
add_library(${C_TESTLIB_STRESS} STATIC ${UnitTestCommon})
add_dependencies(${C_TESTLIB_STRESS} ${PROJECT_NAME}-jet-static)

target_include_directories(${C_TESTLIB_STRESS} PRIVATE
    ${PROJECT_SOURCE_DIR}/test/include)
target_compile_options(${C_TESTLIB_STRESS} PRIVATE
    "${COMMON_TESTLIB_CC_FLAGS}")
target_compile_definitions(${C_TESTLIB_STRESS} PRIVATE
    "${C_TESTLIB_STRESS_DEF}")

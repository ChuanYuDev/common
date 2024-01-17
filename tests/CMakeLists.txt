enable_testing()

set(TEST_LIB test_lib)

add_library(${TEST_LIB} INTERFACE)
target_include_directories(
    ${TEST_LIB} INTERFACE 
    ${COMMON_PUBLIC_HEADER}
)

set(Boost_USE_STATIC_LIBS ON)
find_package(Boost COMPONENTS unit_test_framework REQUIRED)

target_link_libraries(
    ${TEST_LIB} INTERFACE
    ${Boost_UNIT_TEST_FRAMEWORK_LIBRARY}
)

function(add_common_test TEST_NAME TEST_SOURCE)
    add_executable(${TEST_NAME} ${TEST_SOURCE})
    target_link_libraries(${TEST_NAME} ${TEST_LIB})
    add_test(NAME ${TEST_NAME} COMMAND ${TEST_NAME} WORKING_DIRECTORY ${CMAKE_SOURCE_DIR})
endfunction()

add_common_test(test_compare test_compare.cpp)
add_common_test(test_output test_output.cpp)
cmake_minimum_required(VERSION 3.0)

project(CatchSelfTest)

option(USE_VALGRIND "Perform SelfTests with Valgrind" OFF)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# define some folders
set(CATCH_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(SELF_TEST_DIR ${CATCH_DIR}/projects/SelfTest)
set(BENCHMARK_DIR ${CATCH_DIR}/projects/Benchmark)
set(HEADER_DIR ${CATCH_DIR}/include)
set(CATCH_VERSION_NUMBER 2.0.1)

if(USE_WMAIN)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /ENTRY:wmainCRTStartup")
endif()

#checks that the given hard-coded list contains all headers + sources in the given folder
function(CheckFileList LIST_VAR FOLDER)
  set(MESSAGE " should be added to the variable ${LIST_VAR}")
  set(MESSAGE "${MESSAGE} in ${CMAKE_CURRENT_LIST_FILE}\n")
  file(GLOB GLOBBED_LIST "${FOLDER}/*.cpp"
                         "${FOLDER}/*.hpp"
                         "${FOLDER}/*.h")
  list(REMOVE_ITEM GLOBBED_LIST ${${LIST_VAR}})
  foreach(EXTRA_ITEM ${GLOBBED_LIST})
    string(REPLACE "${CATCH_DIR}/" "" RELATIVE_FILE_NAME "${EXTRA_ITEM}")
    message(AUTHOR_WARNING "The file \"${RELATIVE_FILE_NAME}\"${MESSAGE}")
  endforeach()
endfunction()

function(CheckFileListRec LIST_VAR FOLDER)
  set(MESSAGE " should be added to the variable ${LIST_VAR}")
  set(MESSAGE "${MESSAGE} in ${CMAKE_CURRENT_LIST_FILE}\n")
  file(GLOB_RECURSE GLOBBED_LIST "${FOLDER}/*.cpp"
                                 "${FOLDER}/*.hpp"
                                 "${FOLDER}/*.h")
  list(REMOVE_ITEM GLOBBED_LIST ${${LIST_VAR}})
  foreach(EXTRA_ITEM ${GLOBBED_LIST})
    string(REPLACE "${CATCH_DIR}/" "" RELATIVE_FILE_NAME "${EXTRA_ITEM}")
    message(AUTHOR_WARNING "The file \"${RELATIVE_FILE_NAME}\"${MESSAGE}")
  endforeach()
endfunction()

# define the sources of the self test
# Please keep these ordered alphabetically
set(TEST_SOURCES
        ${SELF_TEST_DIR}/TestMain.cpp
        ${SELF_TEST_DIR}/IntrospectiveTests/CmdLine.tests.cpp
        ${SELF_TEST_DIR}/IntrospectiveTests/PartTracker.tests.cpp
        ${SELF_TEST_DIR}/IntrospectiveTests/TagAlias.tests.cpp
        ${SELF_TEST_DIR}/IntrospectiveTests/String.tests.cpp
        ${SELF_TEST_DIR}/IntrospectiveTests/Xml.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Approx.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/BDD.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Benchmark.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Class.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Compilation.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Condition.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Decomposition.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/EnumToString.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Exception.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Message.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Misc.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/ToStringChrono.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/ToStringGeneral.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/ToStringPair.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/ToStringTuple.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/ToStringVector.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/ToStringWhich.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Tricky.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/VariadicMacros.tests.cpp
        ${SELF_TEST_DIR}/UsageTests/Matchers.tests.cpp
        )
CheckFileList(TEST_SOURCES ${SELF_TEST_DIR})

# A set of impl files that just #include a single header
# Please keep these ordered alphabetically
set(SURROGATE_SOURCES
        ${SELF_TEST_DIR}/SurrogateCpps/catch_console_colour.cpp
        ${SELF_TEST_DIR}/SurrogateCpps/catch_debugger.cpp
        ${SELF_TEST_DIR}/SurrogateCpps/catch_interfaces_reporter.cpp
        ${SELF_TEST_DIR}/SurrogateCpps/catch_option.cpp
        ${SELF_TEST_DIR}/SurrogateCpps/catch_stream.cpp
        ${SELF_TEST_DIR}/SurrogateCpps/catch_test_case_tracker.cpp
        ${SELF_TEST_DIR}/SurrogateCpps/catch_test_spec.cpp
        ${SELF_TEST_DIR}/SurrogateCpps/catch_xmlwriter.cpp
        )
CheckFileList(SURROGATE_SOURCES ${SELF_TEST_DIR}/SurrogateCpps)


# Please keep these ordered alphabetically
set(TOP_LEVEL_HEADERS
        ${HEADER_DIR}/catch.hpp
        ${HEADER_DIR}/catch_with_main.hpp
        )
CheckFileList(TOP_LEVEL_HEADERS ${HEADER_DIR})

# Please keep these ordered alphabetically
set(EXTERNAL_HEADERS
        ${HEADER_DIR}/external/clara.hpp
        )
CheckFileList(EXTERNAL_HEADERS ${HEADER_DIR}/external)


# Please keep these ordered alphabetically
set(INTERNAL_HEADERS
        ${HEADER_DIR}/internal/catch_approx.h
        ${HEADER_DIR}/internal/catch_assertionhandler.h
        ${HEADER_DIR}/internal/catch_assertioninfo.h
        ${HEADER_DIR}/internal/catch_assertionresult.h
        ${HEADER_DIR}/internal/catch_capture.hpp
        ${HEADER_DIR}/internal/catch_capture_matchers.h
        ${HEADER_DIR}/internal/catch_clara.h
        ${HEADER_DIR}/internal/catch_commandline.h
        ${HEADER_DIR}/internal/catch_common.h
        ${HEADER_DIR}/internal/catch_compiler_capabilities.h
        ${HEADER_DIR}/internal/catch_config.hpp
        ${HEADER_DIR}/internal/catch_console_colour.h
        ${HEADER_DIR}/internal/catch_context.h
        ${HEADER_DIR}/internal/catch_debug_console.h
        ${HEADER_DIR}/internal/catch_debugger.h
        ${HEADER_DIR}/internal/catch_decomposer.h
        ${HEADER_DIR}/internal/catch_default_main.hpp
        ${HEADER_DIR}/internal/catch_enforce.h
        ${HEADER_DIR}/internal/catch_errno_guard.h
        ${HEADER_DIR}/internal/catch_exception_translator_registry.h
        ${HEADER_DIR}/internal/catch_external_interfaces.h
        ${HEADER_DIR}/internal/catch_fatal_condition.h
        ${HEADER_DIR}/internal/catch_impl.hpp
        ${HEADER_DIR}/internal/catch_interfaces_capture.h
        ${HEADER_DIR}/internal/catch_interfaces_config.h
        ${HEADER_DIR}/internal/catch_interfaces_exception.h
        ${HEADER_DIR}/internal/catch_interfaces_registry_hub.h
        ${HEADER_DIR}/internal/catch_interfaces_reporter.h
        ${HEADER_DIR}/internal/catch_interfaces_runner.h
        ${HEADER_DIR}/internal/catch_interfaces_tag_alias_registry.h
        ${HEADER_DIR}/internal/catch_interfaces_testcase.h
        ${HEADER_DIR}/internal/catch_leak_detector.h
        ${HEADER_DIR}/internal/catch_list.h
        ${HEADER_DIR}/internal/catch_matchers.h
        ${HEADER_DIR}/internal/catch_matchers_floating.h
        ${HEADER_DIR}/internal/catch_matchers_string.h
        ${HEADER_DIR}/internal/catch_matchers_vector.h
        ${HEADER_DIR}/internal/catch_message.h
        ${HEADER_DIR}/internal/catch_objc.hpp
        ${HEADER_DIR}/internal/catch_objc_arc.hpp
        ${HEADER_DIR}/internal/catch_option.hpp
        ${HEADER_DIR}/internal/catch_platform.h
        ${HEADER_DIR}/internal/catch_random_number_generator.h
        ${HEADER_DIR}/internal/catch_reenable_warnings.h
        ${HEADER_DIR}/internal/catch_reporter_registrars.hpp
        ${HEADER_DIR}/internal/catch_reporter_registry.h
        ${HEADER_DIR}/internal/catch_result_type.h
        ${HEADER_DIR}/internal/catch_run_context.h
        ${HEADER_DIR}/internal/catch_benchmark.h
        ${HEADER_DIR}/internal/catch_section.h
        ${HEADER_DIR}/internal/catch_section_info.h
        ${HEADER_DIR}/internal/catch_session.h
        ${HEADER_DIR}/internal/catch_startup_exception_registry.h
        ${HEADER_DIR}/internal/catch_stream.h
        ${HEADER_DIR}/internal/catch_stringref.h
        ${HEADER_DIR}/internal/catch_string_manip.h
        ${HEADER_DIR}/internal/catch_suppress_warnings.h
        ${HEADER_DIR}/internal/catch_tag_alias.h
        ${HEADER_DIR}/internal/catch_tag_alias_autoregistrar.h
        ${HEADER_DIR}/internal/catch_tag_alias_registry.h
        ${HEADER_DIR}/internal/catch_test_case_info.h
        ${HEADER_DIR}/internal/catch_test_case_registry_impl.h
        ${HEADER_DIR}/internal/catch_test_case_tracker.h
        ${HEADER_DIR}/internal/catch_test_registry.h
        ${HEADER_DIR}/internal/catch_test_spec.h
        ${HEADER_DIR}/internal/catch_test_spec_parser.h
        ${HEADER_DIR}/internal/catch_text.h
        ${HEADER_DIR}/internal/catch_timer.h
        ${HEADER_DIR}/internal/catch_tostring.h
        ${HEADER_DIR}/internal/catch_totals.h
        ${HEADER_DIR}/internal/catch_version.h
        ${HEADER_DIR}/internal/catch_wildcard_pattern.h
        ${HEADER_DIR}/internal/catch_windows_h_proxy.h
        ${HEADER_DIR}/internal/catch_xmlwriter.h
        )
set(IMPL_SOURCES
        ${HEADER_DIR}/internal/catch_approx.cpp
        ${HEADER_DIR}/internal/catch_assertionhandler.cpp
        ${HEADER_DIR}/internal/catch_assertionresult.cpp
        ${HEADER_DIR}/internal/catch_benchmark.cpp
        ${HEADER_DIR}/internal/catch_capture_matchers.cpp
        ${HEADER_DIR}/internal/catch_commandline.cpp
        ${HEADER_DIR}/internal/catch_common.cpp
        ${HEADER_DIR}/internal/catch_config.cpp
        ${HEADER_DIR}/internal/catch_console_colour.cpp
        ${HEADER_DIR}/internal/catch_context.cpp
        ${HEADER_DIR}/internal/catch_debug_console.cpp
        ${HEADER_DIR}/internal/catch_debugger.cpp
        ${HEADER_DIR}/internal/catch_decomposer.cpp
        ${HEADER_DIR}/internal/catch_errno_guard.cpp
        ${HEADER_DIR}/internal/catch_exception_translator_registry.cpp
        ${HEADER_DIR}/internal/catch_fatal_condition.cpp
        ${HEADER_DIR}/internal/catch_interfaces_capture.cpp
        ${HEADER_DIR}/internal/catch_interfaces_config.cpp
        ${HEADER_DIR}/internal/catch_interfaces_exception.cpp
        ${HEADER_DIR}/internal/catch_interfaces_registry_hub.cpp
        ${HEADER_DIR}/internal/catch_interfaces_runner.cpp
        ${HEADER_DIR}/internal/catch_interfaces_testcase.cpp
        ${HEADER_DIR}/internal/catch_list.cpp
        ${HEADER_DIR}/internal/catch_leak_detector.cpp
        ${HEADER_DIR}/internal/catch_matchers.cpp
        ${HEADER_DIR}/internal/catch_matchers_floating.cpp
        ${HEADER_DIR}/internal/catch_matchers_string.cpp
        ${HEADER_DIR}/internal/catch_message.cpp
        ${HEADER_DIR}/internal/catch_registry_hub.cpp
        ${HEADER_DIR}/internal/catch_interfaces_reporter.cpp
        ${HEADER_DIR}/internal/catch_random_number_generator.cpp
        ${HEADER_DIR}/internal/catch_reporter_registry.cpp
        ${HEADER_DIR}/internal/catch_result_type.cpp
        ${HEADER_DIR}/internal/catch_run_context.cpp
        ${HEADER_DIR}/internal/catch_section.cpp
        ${HEADER_DIR}/internal/catch_section_info.cpp
        ${HEADER_DIR}/internal/catch_session.cpp
        ${HEADER_DIR}/internal/catch_startup_exception_registry.cpp
        ${HEADER_DIR}/internal/catch_stream.cpp
        ${HEADER_DIR}/internal/catch_stringref.cpp
        ${HEADER_DIR}/internal/catch_string_manip.cpp
        ${HEADER_DIR}/internal/catch_tag_alias.cpp
        ${HEADER_DIR}/internal/catch_tag_alias_autoregistrar.cpp
        ${HEADER_DIR}/internal/catch_tag_alias_registry.cpp
        ${HEADER_DIR}/internal/catch_test_case_info.cpp
        ${HEADER_DIR}/internal/catch_test_case_registry_impl.cpp
        ${HEADER_DIR}/internal/catch_test_case_tracker.cpp
        ${HEADER_DIR}/internal/catch_test_registry.cpp
        ${HEADER_DIR}/internal/catch_test_spec.cpp
        ${HEADER_DIR}/internal/catch_test_spec_parser.cpp
        ${HEADER_DIR}/internal/catch_timer.cpp
        ${HEADER_DIR}/internal/catch_tostring.cpp
        ${HEADER_DIR}/internal/catch_totals.cpp
        ${HEADER_DIR}/internal/catch_version.cpp
        ${HEADER_DIR}/internal/catch_wildcard_pattern.cpp
        ${HEADER_DIR}/internal/catch_xmlwriter.cpp
        )
set(INTERNAL_FILES ${IMPL_SOURCES} ${INTERNAL_HEADERS})
CheckFileList(INTERNAL_FILES ${HEADER_DIR}/internal)

# Please keep these ordered alphabetically
set(REPORTER_HEADERS
        ${HEADER_DIR}/reporters/catch_reporter_automake.hpp
        ${HEADER_DIR}/reporters/catch_reporter_bases.hpp
        ${HEADER_DIR}/reporters/catch_reporter_compact.h
        ${HEADER_DIR}/reporters/catch_reporter_console.h
        ${HEADER_DIR}/reporters/catch_reporter_junit.h
        ${HEADER_DIR}/reporters/catch_reporter_multi.h
        ${HEADER_DIR}/reporters/catch_reporter_tap.hpp
        ${HEADER_DIR}/reporters/catch_reporter_teamcity.hpp
        ${HEADER_DIR}/reporters/catch_reporter_xml.h
        )
set(REPORTER_SOURCES
        ${HEADER_DIR}/reporters/catch_reporter_bases.cpp
        ${HEADER_DIR}/reporters/catch_reporter_compact.cpp
        ${HEADER_DIR}/reporters/catch_reporter_console.cpp
        ${HEADER_DIR}/reporters/catch_reporter_junit.cpp
        ${HEADER_DIR}/reporters/catch_reporter_multi.cpp
        ${HEADER_DIR}/reporters/catch_reporter_xml.cpp
        )
set(REPORTER_FILES ${REPORTER_HEADERS} ${REPORTER_SOURCES})
CheckFileList(REPORTER_FILES ${HEADER_DIR}/reporters)

# Specify the headers, too, so CLion recognises them as project files
set(HEADERS
        ${TOP_LEVEL_HEADERS}
        ${EXTERNAL_HEADERS}
        ${INTERNAL_HEADERS}
        ${REPORTER_HEADERS}
        )

# Provide some groupings for IDEs
SOURCE_GROUP("Tests" FILES ${TEST_SOURCES})
SOURCE_GROUP("Surrogates" FILES ${SURROGATE_SOURCES})

# configure the executable

# Projects consuming Catch via ExternalProject_Add might want to use install step
# without building all of our selftests.
if (NOT NO_SELFTEST)
    add_executable(SelfTest ${TEST_SOURCES} ${IMPL_SOURCES} ${REPORTER_SOURCES} ${SURROGATE_SOURCES} ${HEADERS})
    target_include_directories(SelfTest PRIVATE ${HEADER_DIR})

    if(USE_CPP14)
        message(STATUS "Enabling C++14")
        set_property(TARGET SelfTest PROPERTY CXX_STANDARD 14)
    else()
        message(STATUS "Enabling C++11")
        set_property(TARGET SelfTest PROPERTY CXX_STANDARD 11)
    endif()

    set_property(TARGET SelfTest PROPERTY CXX_STANDARD_REQUIRED ON)
    set_property(TARGET SelfTest PROPERTY CXX_EXTENSIONS OFF)


    # Add desired warnings
    if ( CMAKE_CXX_COMPILER_ID MATCHES "Clang|AppleClang|GNU" )
        target_compile_options( SelfTest PRIVATE -Wall -Wextra -Wunreachable-code )
    endif()
    # Clang specific warning go here
    if ( CMAKE_CXX_COMPILER_ID MATCHES "Clang" )
        # Actually keep these
        target_compile_options( SelfTest PRIVATE -Wweak-vtables -Wexit-time-destructors -Wglobal-constructors -Wmissing-noreturn )
    endif()
    if ( CMAKE_CXX_COMPILER_ID MATCHES "MSVC" )
        target_compile_options( SelfTest PRIVATE /W4 /w44265 /WX /w44061 /w44062 )
    endif()


    # configure unit tests via CTest
    enable_testing()
    add_test(NAME RunTests COMMAND $<TARGET_FILE:SelfTest>)

    add_test(NAME ListTests COMMAND $<TARGET_FILE:SelfTest> --list-tests)
    set_tests_properties(ListTests PROPERTIES PASS_REGULAR_EXPRESSION "[0-9]+ test cases")

    add_test(NAME ListTags COMMAND $<TARGET_FILE:SelfTest> --list-tags)
    set_tests_properties(ListTags PROPERTIES PASS_REGULAR_EXPRESSION "[0-9]+ tags")

    # AppVeyor has a Python 2.7 in path, but doesn't have .py files as autorunnable
    add_test(NAME ApprovalTests COMMAND python ${CMAKE_CURRENT_SOURCE_DIR}/scripts/approvalTests.py $<TARGET_FILE:SelfTest>)
    set_tests_properties(ApprovalTests PROPERTIES FAIL_REGULAR_EXPRESSION "Results differed")
    
    if (USE_VALGRIND)
        add_test(NAME ValgrindRunTests COMMAND valgrind --leak-check=full --error-exitcode=1 $<TARGET_FILE:SelfTest>)
        add_test(NAME ValgrindListTests COMMAND valgrind --leak-check=full --error-exitcode=1 $<TARGET_FILE:SelfTest> --list-tests)
        set_tests_properties(ValgrindListTests PROPERTIES PASS_REGULAR_EXPRESSION "definitely lost: 0 bytes in 0 blocks")
        add_test(NAME ValgrindListTags COMMAND valgrind --leak-check=full --error-exitcode=1 $<TARGET_FILE:SelfTest> --list-tags)
        set_tests_properties(ValgrindListTags PROPERTIES PASS_REGULAR_EXPRESSION "definitely lost: 0 bytes in 0 blocks")
    endif()
    
endif() # !NO_SELFTEST


if(BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()


install(DIRECTORY "single_include/" DESTINATION "include/catch")

## Provide some pkg-config integration
# Don't bother on Windows
if(NOT WIN32 OR NOT CMAKE_HOST_SYSTEM_NAME MATCHES Windows)

    set(PKGCONFIG_INSTALL_DIR
        "${CMAKE_INSTALL_PREFIX}/share/pkgconfig"
        CACHE PATH "Path where catch.pc is installed"
    )

    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/catch.pc.in ${CMAKE_CURRENT_BINARY_DIR}/catch.pc @ONLY)
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/catch.pc DESTINATION ${PKGCONFIG_INSTALL_DIR})

endif()

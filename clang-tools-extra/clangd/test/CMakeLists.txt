set(CLANGD_TEST_DEPS
  clangd
  ClangdTests
  # No tests for these, but we should still make sure they build.
  clangd-indexer
  dexp
  )

if(CLANGD_BUILD_XPC)
  list(APPEND CLANGD_TEST_DEPS clangd-xpc-test-client)
  list(APPEND CLANGD_TEST_DEPS ClangdXpcUnitTests)
endif()

foreach(dep FileCheck count not)
  if(TARGET ${dep})
    list(APPEND CLANGD_TEST_DEPS ${dep})
  endif()
endforeach()

configure_lit_site_cfg(
  ${CMAKE_CURRENT_SOURCE_DIR}/lit.cfg.in
  ${CMAKE_CURRENT_BINARY_DIR}/lit.cfg)

add_lit_testsuite(check-clangd "Running the Clangd regression tests"
  ${CMAKE_CURRENT_BINARY_DIR}/../unittests;${CMAKE_CURRENT_BINARY_DIR}
  DEPENDS ${CLANGD_TEST_DEPS})

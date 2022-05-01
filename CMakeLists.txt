cmake_minimum_required (VERSION 3.11)

add_subdirectory(third_party)

project(smk-starter
  LANGUAGES CXX
  VERSION 1.0.0
)

add_executable(smk-starter
  src/main.cpp
)

target_include_directories(smk-starter PRIVATE src)
target_link_libraries(smk-starter PRIVATE smk)

set_target_properties(smk-starter PROPERTIES CXX_STANDARD 11)

target_compile_options(smk-starter PRIVATE -Wall -Wextra -pedantic -Werror)

# Detect emscripten is used.
if(EMSCRIPTEN)
  set_property(TARGET smk-starter APPEND_STRING PROPERTY LINK_FLAGS  " -s WASM=1")

  # Copy the index.html file.
  configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/src/index.html
    ${CMAKE_CURRENT_BINARY_DIR}/index.html
    COPYONLY
  )

  # Copy the run_webassembly.py file.
  configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/src/run_webassembly.py
    ${CMAKE_CURRENT_BINARY_DIR}/run_webassembly.py
    COPYONLY
  )
endif()

install(TARGETS smk-starter RUNTIME DESTINATION "bin")

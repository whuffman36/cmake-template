cmake_minimum_required(VERSION 3.28)

project(Main VERSION 1.0)

# Set for Apple Silicon.
set(CMAKE_HOST_SYSTEM_PROCESSOR arm64)
# C++17.
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)
# Compiler variables to set flags.
set(gcc_like_cxx "$<COMPILE_LANG_AND_ID:CXX,ARMClang,AppleClang,Clang,GNU,LCC>")
set(msvc_cxx "$<COMPILE_LANG_AND_ID:CXX,MSVC>")
# clangd interopability.
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

add_subdirectory(src/lib)
add_executable(Main src/main.cpp)

# Link libraries.
target_link_libraries(Main PUBLIC Lib)

# Link header files.
target_include_directories(Main PUBLIC
                          "${PROJECT_BINARY_DIR}"
                          "${PROJECT_SOURCE_DIR}/lib"
                          )

# Compiler flags.
target_compile_options(Main PUBLIC
  "$<${gcc_like_cxx}:-Wall;-Wextra;-pedantic;-Wunused>"
  "$<${msvc_cxx}:-W3>>")

cmake_minimum_required(VERSION 3.22.1)

project(nodeunta_server)

# set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra")
# set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -g -fsanitize=address")
# set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fsanitize=address")

find_package(PkgConfig REQUIRED)
pkg_check_modules(YAML REQUIRED yaml-0.1)

set(HEADER_DIR include)

file(GLOB headers "${HEADER_DIR}/*.h")
file(GLOB sources "src/*.c")

add_library(custom STATIC ${sources} ${headers})

target_compile_options(custom PRIVATE -Wall -Wextra)

target_include_directories(custom PUBLIC "${HEADER_DIR}" ${YAML_INCLUDE_DIRS})

target_compile_definitions(custom PRIVATE
  $<$<CONFIG:Debug>:DEBUG_BUILD>
)

add_executable(${CMAKE_PROJECT_NAME} main.c)

target_compile_options(${CMAKE_PROJECT_NAME} PRIVATE -Wall -Wextra)

target_compile_definitions(${CMAKE_PROJECT_NAME} PRIVATE
  $<$<CONFIG:Debug>:DEBUG_BUILD>
)

target_link_libraries(${CMAKE_PROJECT_NAME} custom ${YAML_LIBRARIES})

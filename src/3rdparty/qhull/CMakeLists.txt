cmake_minimum_required(VERSION 2.6)

file(GLOB qhull_files ${CMAKE_CURRENT_SOURCE_DIR}/*.c)
add_library(qhull STATIC ${qhull_files})
set_target_properties(qhull PROPERTIES COMPILE_FLAGS "${EXTRA_COMPILE_FLAGS}")

install(TARGETS qhull DESTINATION lib)
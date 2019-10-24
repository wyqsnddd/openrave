file(GLOB openrave_cpp_files "*.cpp")
file(GLOB openrave_h_files "*.h")
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/..)
add_library(openrave SHARED ${openrave_cpp_files})
set_target_properties(openrave PROPERTIES
  COMPILE_FLAGS "${CMAKE_CXX_FLAGS_DEBUG}"
    INSTALL_RPATH_USE_LINK_PATH TRUE CLEAN_DIRECT_OUTPUT 1
    VERSION ${OPENRAVE_VERSION}
)
target_link_libraries(openrave xml2 fparser ${Boost_FILESYSTEM_LIBRARY} ${Boost_THREAD_LIBRARY})

install(TARGETS openrave DESTINATION lib)
install(FILES ${openrave_h_files} DESTINATION include/openrave)
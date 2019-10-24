cmake_minimum_required(VERSION 2.8.9)
project(bindings)
set(CMAKE_CXX_STANDARD 11)
find_package (Threads REQUIRED)

find_package(Python3 COMPONENTS Interpreter Development)
message(STATUS "Python3_EXECUTABLE        = ${Python3_EXECUTABLE}")
message(STATUS "Python3_INCLUDE_DIR       = ${Python3_INCLUDE_DIR}")
message(STATUS "Python3_LIBRARY           = ${Python3_LIBRARY}")
message(STATUS "Python3_SITELIB           = ${Python3_SITELIB}")
message(STATUS "Python3_FOUND             = ${Python3_FOUND}")
# set(Python3_ROOT ${Python3_INCLUDE_DIR}/../..)
# message(STATUS "Python3_ROOT              = ${Python3_ROOT}")

set(Python3_NUMPY_INCLUDE_DIR ${Python3_SITELIB}/numpy/core/include)
set(Python3_NUMPY_LIBRARY_DIR ${Python3_SITELIB}/numpy/core/lib)
message(STATUS "Python3_NUMPY_INCLUDE_DIR = ${Python3_NUMPY_INCLUDE_DIR}")
message(STATUS "Python3_NUMPY_LIBRARY_DIR = ${Python3_NUMPY_LIBRARY_DIR}")

message(STATUS "CMAKE_SOURCE_DIR = ${CMAKE_SOURCE_DIR}")

include_directories(
	${BOOST_INCLUDEDIR}  # boost
	${Python3_INCLUDE_DIR} # python3
	${Python3_NUMPY_INCLUDE_DIR} # numpy
)
link_directories(
	${BOOST_LIBRARYDIR} # boost
	# ${Python3_LIBRARY}  # python3
	${BOOST_PYTHON3_LIBRARYDIR} # numpy
)
add_definitions(-DOPENRAVE_BININGS_PYARRAY)

set(libraries_to_link boost_python37 boost_numpy37 dl util ${Python3_LIBRARY} ${CMAKE_THREAD_LIBS_INIT})

add_executable(bindings bindings.cpp)
# set_target_properties(bindings
#     PROPERTIES 
#     COMPILE_FLAGS "-L${CMAKE_SOURCE_DIR}/lib -g -O0"
# )

target_link_libraries(bindings ${libraries_to_link})


cmake_minimum_required(VERSION 2.9)
set(CMAKE_CXX_STANDARD 11)
cmake_policy(SET CMP0046 NEW)

find_package(Boost COMPONENTS REQUIRED regex filesystem system thread iostreams date_time) # python
include_directories(${BOOST_INCLUDEDIR})
link_directories(${BOOST_LIBRARYDIR})

find_package(zlib REQUIRED)
find_package(libxml2 REQUIRED)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../minizip /usr/local/include)
# need to link by
# ln -s /usr/local/Cellar/libxml2/2.9.9_2/include/libxml2/libxml /usr/local/include/libxml

message(STATUS "ZLIB_FOUND             = ${ZLIB_FOUND}")
message(STATUS "Boost_FILESYSTEM_FOUND = ${Boost_FILESYSTEM_FOUND}")
message(STATUS "Boost_SYSTEM_FOUND     = ${Boost_SYSTEM_FOUND}")

if( ZLIB_FOUND AND Boost_FILESYSTEM_FOUND AND Boost_SYSTEM_FOUND )
	message(STATUS "Compiling with COLLADA support from sources!")
	if( LIBXML2_DEFINITIONS )
	  add_definitions(${LIBXML2_DEFINITIONS})
	endif()
	if( LIBXML2_INCLUDE_DIR )
	  include_directories(${LIBXML2_INCLUDE_DIR})
	endif()

	include_directories(${MINIZIP_INCLUDE_DIR})
	include_directories(BEFORE ${libpcrecpp_INCLUDE_DIRS})
	link_directories(${libpcrecpp_LIBRARY_DIRS})

	include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include ${CMAKE_CURRENT_SOURCE_DIR}/include/1.5)
	file(GLOB dom_files1_4 ${CMAKE_CURRENT_SOURCE_DIR}/src/1.4/dom/*.cpp)
	file(GLOB dom_files1_5 ${CMAKE_CURRENT_SOURCE_DIR}/src/1.5/dom/*.cpp)
	file(GLOB dae_files ${CMAKE_CURRENT_SOURCE_DIR}/src/dae/*.cpp)
	file(GLOB libxmlplugin_files ${CMAKE_CURRENT_SOURCE_DIR}/src/modules/LIBXMLPlugin/*.cpp)
	file(GLOB stddatabase_files ${CMAKE_CURRENT_SOURCE_DIR}/src/modules/STLDatabase/*.cpp)
	file(GLOB stderrplugin_files ${CMAKE_CURRENT_SOURCE_DIR}/src/modules/stdErrPlugin/*.cpp)

	set(COLLADA_LIBS minizip ${libpcrecpp_LIBRARIES} ${ZLIB_LIBRARIES})
	set(COLLADA_LIBS ${COLLADA_LIBS} ${Boost_FILESYSTEM_LIBRARY} ${Boost_SYSTEM_LIBRARY})

    list(REMOVE_ITEM dae_files ${CMAKE_CURRENT_SOURCE_DIR}/src/dae/daeTinyXMLPlugin.cpp)
	add_library(collada15reader STATIC ${dom_files1_5} ${dae_files} ${libxmlplugin_files} ${stddatabase_files} ${stderrplugin_files})

	if( NOT libpcrecpp_FOUND )
	  add_dependencies(collada15reader pcre_local)
	endif()

	add_dependencies(collada15reader openrave)
	target_link_libraries(collada15reader xml2 pcrecpp ${COLLADA_LIBS})
	set_target_properties(collada15reader PROPERTIES COMPILE_FLAGS "${libpcrecpp_CFLAGS_OTHERS} ${EXTRA_COMPILE_FLAGS} -DDOM_INCLUDE_LIBXML -DCOLLADA_DOM_DAEFLOAT_IS64 -DCOLLADA_DOM_SUPPORT150 ${Boost_CFLAGS}"
	  LINK_FLAGS "${libpcrecpp_LDFLAGS_OTHERS}")

else()
	if( NOT Boost_FILESYSTEM_FOUND )
	  message(STATUS "Need to install boost filesystem")
	endif()
	if( NOT Boost_SYSTEM_FOUND )
	  message(STATUS "Need to install boost system")
	endif()
	if( NOT ZLIB_FOUND )
	  message(STATUS "Need to install zlib")
	endif()
	message(STATUS "No COLLADA support")
endif()

install(TARGETS collada15reader DESTINATION lib)

# set the flags
set(COLLADA_DOM_FOUND 1 PARENT_SCOPE)
set(COLLADA_DOM_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/include PARENT_SCOPE)
set(COLLADA_DOM_LIBRARY_DIRS PARENT_SCOPE)
set(COLLADA_DOM_LIBRARIES collada15reader PARENT_SCOPE)
set(COLLADA_DOM_CFLAGS_OTHER "-DCOLLADA_DOM_SUPPORT150 -DCOLLADA_DOM_DAEFLOAT_IS64" PARENT_SCOPE)
set(COLLADA_DOM_LDFLAGS_OTHER PARENT_SCOPE)

cmake_minimum_required(VERSION 2.8.9)
# before openrave
add_subdirectory(ann)
add_subdirectory(zlib)
add_subdirectory(minizip)
add_subdirectory(fparser-4.5.2)
add_subdirectory(pcre-8.02)

add_subdirectory(convexdecomposition)
add_subdirectory(crlibm-1.0beta4)
add_subdirectory(flann-1.6.6)
add_subdirectory(ivcon)
add_subdirectory(qhull)

# after openrave
# add_subdirectory(collada)

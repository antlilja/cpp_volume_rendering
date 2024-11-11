add_library(file_utils STATIC pvm_old.cpp            pvm_old.h
                              pvm.cpp                pvm.h
                              rawloader.cpp          rawloader.h)

include_directories(${CMAKE_SOURCE_DIR}/include)
add_definitions(-DEXPMODULE)

include_directories(${CMAKE_SOURCE_DIR}/libs)
include_directories(${CMAKE_SOURCE_DIR}/lib)

link_directories(${CMAKE_LIBRARY_OUTPUT_DIRECTORY})

find_package(GLEW REQUIRED)

# only link with math_utils
target_link_libraries(file_utils debug gl_utils)
target_link_libraries(file_utils debug ${GLEW_LIBRARIES})

target_link_libraries(file_utils optimized gl_utils)
target_link_libraries(file_utils optimized ${GLEW_LIBRARIES})
                      
# add dependency
add_dependencies(file_utils gl_utils)

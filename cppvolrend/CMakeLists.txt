include_directories(${CMAKE_SOURCE_DIR}/include)
include_directories(${CMAKE_SOURCE_DIR}/libs)
include_directories(${CMAKE_SOURCE_DIR}/external)
include_directories(${CMAKE_SOURCE_DIR}/external/imgui)
include_directories(${CMAKE_SOURCE_DIR}/external/imgui/examples)

#message(${CMAKE_SOURCE_DIR})
link_directories(${CMAKE_LIBRARY_OUTPUT_DIRECTORY})
link_directories(${CMAKE_SOURCE_DIR}/lib)

add_definitions(-DCMAKE_PATH_OUTPUT_DIR=${CMAKE_LIBRARY_OUTPUT_DIRECTORY})

set(PATH_TO_APP_FOLDER ${CMAKE_SOURCE_DIR}/cppvolrend/)
add_definitions(-DCMAKE_PATH_TO_APP_FOLDER=${PATH_TO_APP_FOLDER})

set(PATH_TO_INCLUDE ${CMAKE_SOURCE_DIR}/include/)
add_definitions(-DCMAKE_PATH_TO_INCLUDE=${PATH_TO_INCLUDE})

set(PATH_TO_DATA_FOLDER ${CMAKE_SOURCE_DIR}/data/)
add_definitions(-DCMAKE_PATH_TO_DATA_FOLDER=${PATH_TO_DATA_FOLDER})

# add the executable to be available at ide
add_executable(cppvolrend
               main.cpp                                                        defines.h
               app_freeglut.cpp                                                app_freeglut.h
               app_glfw.cpp                                                    app_glfw.h
               renderingmanager.cpp                                            renderingmanager.h
               volrenderbase.cpp                                               volrenderbase.h
               
               # Null Bounding Box Grid
               volrendernull.cpp                                               volrendernull.h
              
               # GPU Image Order Ray Casting
               structured/rc1pass/rc1prenderer.cpp                             structured/rc1pass/rc1prenderer.h

               # GPU Image Order Iso Ray Casting with adaptive step size
               structured/rc1pisoadapt/rc1pisoadaptrenderer.cpp                structured/rc1pisoadapt/rc1pisoadaptrenderer.h

               # Directional Ambient Occlusion and Cone Shadows Ground Truth
               structured/rc1pcrtgt/crtgtrenderer.cpp                          structured/rc1pcrtgt/crtgtrenderer.h

               # Directional Occlusion Shading for Single Pass GPU Ray Casting
               structured/rc1pdosct/conegaussiansampler.cpp                    structured/rc1pdosct/conegaussiansampler.h
               structured/rc1pdosct/dosrcrenderer.cpp                          structured/rc1pdosct/dosrcrenderer.h
               structured/rc1pdosct/extcoefvolumegenerator.cpp                 structured/rc1pdosct/extcoefvolumegenerator.h
              
               # Extinction-based shading to compute ambient occlusion and cone shadows with Summed Area Table
               structured/rc1pextbsd/ebsrenderer.cpp                           structured/rc1pextbsd/ebsrenderer.h
               
               # Voxel Cone Tracing to compute Shadows (Single GPU version)
               structured/rc1pvctsg/preprocessingstages.cpp                    structured/rc1pvctsg/preprocessingstages.h
               structured/rc1pvctsg/vctrenderer.cpp                            structured/rc1pvctsg/vctrenderer.h
               
               # Directional Occlusion Shading for GPU Slice/Texture-based 
               structured/sbtmdos/sbtmdosrenderer.cpp                          structured/sbtmdos/sbtmdosrenderer.h
               structured/sbtmdos/layeredframebufferobject.cpp                 structured/sbtmdos/layeredframebufferobject.h

               utils/preillumination.cpp                                       utils/preillumination.h
               utils/parameterspace.cpp                                        utils/parameterspace.h

               ${CMAKE_EXTERNAL_DIRECTORY}/imgui/imconfig.h                    ${CMAKE_EXTERNAL_DIRECTORY}/imgui/imgui_demo.cpp
               ${CMAKE_EXTERNAL_DIRECTORY}/imgui/imgui.h                       ${CMAKE_EXTERNAL_DIRECTORY}/imgui/imgui.cpp
               ${CMAKE_EXTERNAL_DIRECTORY}/imgui/imgui_draw.cpp                ${CMAKE_EXTERNAL_DIRECTORY}/imgui/imgui_internal.h
               ${CMAKE_EXTERNAL_DIRECTORY}/imgui/imgui_widgets.cpp             ${CMAKE_EXTERNAL_DIRECTORY}/imgui/imstb_rectpack.h
               ${CMAKE_EXTERNAL_DIRECTORY}/imgui/imstb_textedit.h              ${CMAKE_EXTERNAL_DIRECTORY}/imgui/imstb_truetype.h
               ${CMAKE_EXTERNAL_DIRECTORY}/imgui/examples/imgui_impl_glut.h    ${CMAKE_EXTERNAL_DIRECTORY}/imgui/examples/imgui_impl_glut.cpp
               ${CMAKE_EXTERNAL_DIRECTORY}/imgui/examples/imgui_impl_opengl2.h ${CMAKE_EXTERNAL_DIRECTORY}/imgui/examples/imgui_impl_opengl2.cpp
               ${CMAKE_EXTERNAL_DIRECTORY}/imgui/examples/imgui_impl_glfw.h    ${CMAKE_EXTERNAL_DIRECTORY}/imgui/examples/imgui_impl_glfw.cpp
               ${CMAKE_EXTERNAL_DIRECTORY}/imgui/examples/imgui_impl_opengl3.h ${CMAKE_EXTERNAL_DIRECTORY}/imgui/examples/imgui_impl_opengl3.cpp
               )

find_package(PkgConfig REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)
pkg_search_module(GLFW REQUIRED glfw3)
find_package(GLUT REQUIRED)
link_directories(${OPENGL_gl_LIBRARY})

# . Debug
target_link_libraries(cppvolrend debug ${OPENGL_gl_LIBRARY})
target_link_libraries(cppvolrend debug ${GLUT_LIBRARIES})
target_link_libraries(cppvolrend debug ${GLFW_LIBRARIES})
target_link_libraries(cppvolrend debug ${GLEW_LIBRARIES})
target_link_libraries(cppvolrend debug file_utils)
target_link_libraries(cppvolrend debug gl_utils)
target_link_libraries(cppvolrend debug math_utils)
target_link_libraries(cppvolrend debug vis_utils)
target_link_libraries(cppvolrend debug volvis_utils)
# . Release
target_link_libraries(cppvolrend optimized ${OPENGL_gl_LIBRARY})
target_link_libraries(cppvolrend optimized ${GLUT_LIBRARIES})
target_link_libraries(cppvolrend optimized ${GLFW_LIBRARIES})
target_link_libraries(cppvolrend optimized ${GLEW_LIBRARIES})
target_link_libraries(cppvolrend optimized file_utils)
target_link_libraries(cppvolrend optimized gl_utils)
target_link_libraries(cppvolrend optimized math_utils)
target_link_libraries(cppvolrend optimized vis_utils)
target_link_libraries(cppvolrend optimized volvis_utils)

# add dependency
add_dependencies(cppvolrend file_utils)
add_dependencies(cppvolrend gl_utils)
add_dependencies(cppvolrend math_utils)
add_dependencies(cppvolrend vis_utils)
add_dependencies(cppvolrend volvis_utils)

project(sibr_core)
add_subdirectory(assets)
add_subdirectory(graphics)
add_subdirectory(imgproc)
add_subdirectory(raycaster)
add_subdirectory(renderer)
add_subdirectory(scene)
add_subdirectory(system)
add_subdirectory(video)
add_subdirectory(view)

# SIBR_CORE
file(GLOB_RECURSE assetsheader "assets/*.hpp" "assets/*.h")
file(GLOB_RECURSE graphicsheader "graphics/*.hpp" "graphics/*.h" "graphics/*.inl")
file(GLOB_RECURSE imgprocheader "imgproc/*.hpp" "imgproc/*.h")
file(GLOB_RECURSE raycasterheader "raycaster/*.hpp" "raycaster/*.h")
file(GLOB_RECURSE rendererheader "renderer/*.hpp" "renderer/*.h")
file(GLOB_RECURSE sceneheader "scene/*.hpp" "scene/*.h")
file(GLOB_RECURSE systemheader "system/*.hpp" "system/*.h")
file(GLOB_RECURSE videoheader "video/*.hpp" "video/*.h")
file(GLOB_RECURSE viewheader "view/*.hpp" "view/*.h")

# ExtLibs
file(GLOB_RECURSE picojsonheader "${CMAKE_CURRENT_SOURCE_DIR}/../../extlibs/picojson/*.hpp")
file(GLOB_RECURSE imguiheader "${CMAKE_CURRENT_SOURCE_DIR}/../../extlibs/imgui/*.h")

add_library(${PROJECT_NAME} INTERFACE)


target_include_directories(${PROJECT_NAME} INTERFACE
  $<INSTALL_INTERFACE:include>
  $<INSTALL_INTERFACE:include/extlibs>
  )
target_link_directories(${PROJECT_NAME} INTERFACE
  $<INSTALL_INTERFACE:lib>)

include(GNUInstallDirs)

set(SIBR_CORE_LIBRARIES 
"libimgui.so"
"libmrf.so"
"libnativefiledialog.a"
"libxatlas.so"
"libsibr_system.so"
"libsibr_graphics.so"
"libsibr_assets.so"
"libsibr_raycaster.so"
"libsibr_video.so"
"libsibr_imgproc.so"
"libsibr_view.so"
"libsibr_scene.so"
"libsibr_renderer.so"
)

set_target_properties(${PROJECT_NAME} PROPERTIES
  INTERFACE_LINK_LIBRARIES "${SIBR_CORE_LIBRARIES}")

# install
install(FILES ${assetsheader} DESTINATION include/core/assets)
install(FILES ${graphicsheader} DESTINATION include/core/graphics)
install(FILES ${imgprocheader} DESTINATION include/core/imgproc)
install(FILES ${raycasterheader} DESTINATION include/core/raycaster)
install(FILES ${rendererheader} DESTINATION include/core/renderer)
install(FILES ${sceneheader} DESTINATION include/core/scene)
install(FILES ${systemheader} DESTINATION include/core/system)
install(FILES ${videoheader} DESTINATION include/core/video)
install(FILES ${viewheader} DESTINATION include/core/view)
install(FILES ${picojsonheader} DESTINATION include/extlibs/picojson)
install(FILES ${imguiheader} DESTINATION include/extlibs/imgui)


install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}Targets)
install(EXPORT ${PROJECT_NAME}Targets DESTINATION share/cmake/${PROJECT_NAME})

configure_file(${PROJECT_NAME}.config.cmake.in ${PROJECT_NAME}Config.cmake @ONLY)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake DESTINATION share/cmake/${PROJECT_NAME})
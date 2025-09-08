cmake_minimum_required(VERSION 3.26.0)
cmake_policy(VERSION 3.26.0)


target_sources(wave_tracer PRIVATE
    miniply/miniply.cpp

    tinyexpr-plusplus/tinyexpr.cpp
)


set(BUILD_SHARED_LIBS OFF CACHE BOOL " " FORCE)


add_subdirectory(sockpp)

set(MP_UNITS_API_CONTRACTS NONE CACHE BOOL " " FORCE)
add_subdirectory(mp-units/src)

set(CERF_CPP ON  CACHE BOOL " " FORCE)
add_subdirectory(libcerf/libcpp)

add_subdirectory(function2)

set(YAML_CPP_BUILD_TESTS OFF CACHE BOOL " " FORCE)
set(YAML_CPP_BUILD_TOOLS OFF CACHE BOOL " " FORCE)
set(YAML_CPP_BUILD_CONTRIB OFF CACHE BOOL " " FORCE)
add_subdirectory(yaml-cpp)

set(PUGIXML_BUILD_TESTS OFF CACHE BOOL " " FORCE)
add_subdirectory(pugixml)

set(BUILD_TESTING OFF CACHE BOOL " " FORCE)
set(OPENEXR_BUILD_TOOLS OFF CACHE BOOL " " FORCE)
set(OPENEXR_INSTALL OFF CACHE BOOL " " FORCE)
set(OPENEXR_INSTALL_EXAMPLES OFF CACHE BOOL " " FORCE)
set(OPENEXR_BUILD_EXAMPLES OFF CACHE BOOL " " FORCE)
add_subdirectory(openexr)


target_link_libraries(wave_tracer PUBLIC
    mp-units::mp-units
    Sockpp::sockpp
    cerfcpp
    yaml-cpp
    pugixml
    OpenEXR::OpenEXR
    OpenEXR::OpenEXRUtil
    function2

    githash
)

target_include_directories(wave_tracer PRIVATE 
    CLI11/include

    thread-pool/include
    sockpp/include

    glm

    indicators/single_include
    magic_enum/include

    tinybvh
    tinyobjloader/
    miniply
    avir
    tinycolormap/include
    gcem/include
    libcerf/lib
    json/include
    
    mp-units/src/core/include
    tinyexpr-plusplus

    yaml-cpp/include
    pugixml/src
    ${CMAKE_BINARY_DIR}/deps/openexr/src/lib  # For generated headers

    GitHash/include
)


# GUI
if(BUILD_GUI)
    target_sources(wave_tracer PRIVATE
        imgui/imgui.cpp
        imgui/imgui_draw.cpp
        imgui/imgui_tables.cpp
        imgui/imgui_widgets.cpp
        imgui/backends/imgui_impl_sdl3.cpp
        imgui/backends/imgui_impl_opengl3.cpp

        imgui_toggle/imgui_toggle.cpp
        imgui_toggle/imgui_toggle_palette.cpp
        imgui_toggle/imgui_toggle_presets.cpp
        imgui_toggle/imgui_toggle_renderer.cpp

        implot/implot.cpp
        implot/implot_items.cpp
    )

    target_include_directories(wave_tracer PRIVATE 
        imgui
        implot
        imgui_toggle
    )
endif(BUILD_GUI)


# build number
add_subdirectory(GitHash)


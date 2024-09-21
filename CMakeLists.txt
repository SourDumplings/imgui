cmake_minimum_required(VERSION 3.0)
project(imgui)

set(IMGUI_DIR ${CMAKE_CURRENT_SOURCE_DIR})

file(GLOB
	IMGUI_HEADER
	${IMGUI_DIR}/*.h
	${IMGUI_DIR}/backends/imgui_impl_sdlrenderer3.h
	${IMGUI_DIR}/backends/imgui_impl_sdl3.h
)

file(GLOB IMGUI_SRC
    ${IMGUI_DIR}/*.cpp
	${IMGUI_DIR}/backends/imgui_impl_sdlrenderer3.cpp
	${IMGUI_DIR}/backends/imgui_impl_sdl3.cpp
)

file(GLOB_RECURSE
	IMGUI_MISC_HEADER
	${IMGUI_DIR}/misc/*.h
)

file(GLOB_RECURSE
	IMGUI_MISC_SRC
	${IMGUI_DIR}/misc/*.cpp
)

message("IMGUI_SRC: " ${IMGUI_SRC})
message("IMGUI_MISC_SRC: " ${IMGUI_MISC_SRC})

include_directories(${external_dir}/SDL/include)
include_directories(${external_dir}/SDL_ttf/external/freetype/include)
include_directories(${external_dir}/imgui)

add_library(imgui STATIC ${IMGUI_SRC} ${IMGUI_MISC_SRC})

target_include_directories(imgui PUBLIC ${IMGUI_HEADER} ${IMGUI_MISC_HEADER})

# install and export
install(TARGETS ${PROJECT_NAME}
	EXPORT imgui
	RUNTIME DESTINATION bin COMPONENT libraries
	ARCHIVE DESTINATION lib${LIB_SUFFIX} COMPONENT libraries
	LIBRARY DESTINATION lib${LIB_SUFFIX} COMPONENT libraries
	PUBLIC_HEADER DESTINATION include/imgui
)

if (MSVC AND ${CMAKE_BUILD_TYPE} MATCHES "Debug")
	# Install .pdb files if using MSVC
	message("Installing pdb file...")
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/Debug/${PROJECT_NAME}.pdb DESTINATION lib${LIB_SUFFIX} CONFIGURATIONS Debug)
endif()
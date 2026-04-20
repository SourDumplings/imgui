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

link_directories(${CMAKE_LIBRARY_PATH})

add_library(imgui STATIC ${IMGUI_SRC})

target_include_directories(imgui PUBLIC ${IMGUI_HEADER})

target_link_libraries(imgui PUBLIC SDL3 SDL3_ttf)

# install and export
install(DIRECTORY ${IMGUI_DIR}/
        DESTINATION ${CMAKE_INCLUDE_PATH}/imgui
        FILES_MATCHING PATTERN "*.h"
)

install(TARGETS ${PROJECT_NAME}
	EXPORT imgui
	RUNTIME DESTINATION bin COMPONENT libraries
	ARCHIVE DESTINATION lib${LIB_SUFFIX} COMPONENT libraries
	LIBRARY DESTINATION lib${LIB_SUFFIX} COMPONENT libraries
	PUBLIC_HEADER DESTINATION include/imgui
)
cmake_minimum_required(VERSION 3.0)
project(imgui)

set(IMGUI_DIR ${CMAKE_CURRENT_SOURCE_DIR})

file(GLOB IMGUI_SRC
    ${IMGUI_DIR}/*.cpp
    ${IMGUI_DIR}/*.h
)

add_library(imgui STATIC ${IMGUI_SRC})

target_include_directories(imgui PUBLIC ${IMGUI_DIR})

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
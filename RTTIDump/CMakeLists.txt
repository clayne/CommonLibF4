cmake_minimum_required(VERSION 3.20)

include(../cmake/common.cmake)
add_project(
	TARGET_TYPE LIBRARY
	LIBRARY_TYPE SHARED
	PROJECT RTTIDump
	VERSION 1.0.0
	INCLUDE_DIRECTORIES
		src
	GROUPED_FILES
		src/main.cpp
		src/PCH.cpp
		src/PCH.h
		.clang-format
	PRECOMPILED_HEADERS
		src/PCH.h
)

set(Boost_USE_STATIC_LIBS ON)

if (NOT TARGET CommonLibF4)
	add_subdirectory("${CMAKE_CURRENT_SOURCE_DIR}/../CommonLibF4" CommonLibF4)
endif ()

find_package(Boost
	MODULE
	REQUIRED
	COMPONENTS
		regex
)
find_package(robin_hood REQUIRED CONFIG)
find_package(spdlog REQUIRED CONFIG)

target_link_libraries(
	${PROJECT_NAME}
	PUBLIC
		Boost::headers
		Boost::regex
		CommonLibF4::CommonLibF4
		Dbghelp.lib
		robin_hood::robin_hood
		spdlog::spdlog
)

copy_files(
	"$<TARGET_FILE:${PROJECT_NAME}>" "Data/F4SE/Plugins/"
	"$<TARGET_PDB_FILE:${PROJECT_NAME}>" "Data/F4SE/Plugins/"
)

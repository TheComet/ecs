include (TestVisibilityMacros)
include (CheckIncludeFiles)

set (ECS_LIB_TYPE "SHARED" CACHE STRING "Whether the ECS lib should be shared or static")
option (ECS_PIC "Generate position independent code" ON)

# import/export macros
test_visibility_macros (ECS_API_EXPORT ECS_API_IMPORT ECS_API_LOCAL)

# Use stdint.h if possible, otherwise use pstdint.h
check_include_files ("stdint.h" ECS_HAVE_STDINT_H)

configure_file ("templates/config.hpp.in" "include/ecs/config.hpp")

add_library (ecs ${ECS_LIB_TYPE}
	"src/World.cpp")

target_include_directories (ecs
	PUBLIC 
		$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>
		$<INSTALL_INTERFACE:include>)

target_compile_definitions (ecs
	PRIVATE
		ECS_BUILDING)

set_property (TARGET ecs
	PROPERTY POSITION_INDEPENDENT_CODE ${ECS_PIC})

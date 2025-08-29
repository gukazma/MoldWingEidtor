set(TARGET_NAME MoldWingEidtor)


file(GLOB_RECURSE CPP_HEAD_FILES  ${CMAKE_CURRENT_SOURCE_DIR}/*.h)
file(GLOB_RECURSE CPP_SOUCE_FILES  ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)

source_group(TREE ${CMAKE_CURRENT_LIST_DIR} FILES ${CPP_HEAD_FILES} ${CPP_SOUCE_FILES})
find_package(Vulkan REQUIRED)
add_executable(${TARGET_NAME} ${CPP_HEAD_FILES} ${CPP_SOUCE_FILES})
set_target_properties(${TARGET_NAME} PROPERTIES FOLDER Apps)
target_link_libraries(${TARGET_NAME} PRIVATE
imgui
Vulkan::Vulkan
glfw
)

target_include_directories(${TARGET_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/Modules/Shaders/generated/cpp)
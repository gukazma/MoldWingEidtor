set(TARGET_NAME MoldWingEidtor)


file(GLOB_RECURSE CPP_HEAD_FILES  ${CMAKE_CURRENT_SOURCE_DIR}/*.h)
file(GLOB_RECURSE CPP_SOUCE_FILES  ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)

source_group(TREE ${CMAKE_CURRENT_LIST_DIR} FILES ${CPP_HEAD_FILES} ${CPP_SOUCE_FILES})
find_package (Vulkan REQUIRED)

# set up Vulkan C++ module
add_library(VulkanCppModule)
add_library(Vulkan::cppm ALIAS VulkanCppModule)

target_compile_definitions(VulkanCppModule PUBLIC
        VULKAN_HPP_DISPATCH_LOADER_DYNAMIC=1
        VULKAN_HPP_NO_STRUCT_CONSTRUCTORS=1
)
target_include_directories(VulkanCppModule
        PRIVATE
        "${Vulkan_INCLUDE_DIR}"
)
target_link_libraries(VulkanCppModule
        PUBLIC
        Vulkan::Vulkan
)

set_target_properties(VulkanCppModule PROPERTIES CXX_STANDARD 20)

target_sources(VulkanCppModule
        PUBLIC
        FILE_SET cxx_modules TYPE CXX_MODULES
        BASE_DIRS
        "${Vulkan_INCLUDE_DIR}"
        FILES
        "${Vulkan_INCLUDE_DIR}/vulkan/vulkan.cppm"
)

add_executable(${TARGET_NAME} ${CPP_HEAD_FILES} ${CPP_SOUCE_FILES})
set_target_properties(${TARGET_NAME} PROPERTIES FOLDER Apps)
target_link_libraries(${TARGET_NAME} PRIVATE
imgui
Vulkan::cppm
glfw
)
set_target_properties(${TARGET_NAME} PROPERTIES CXX_STANDARD 20)

target_include_directories(${TARGET_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/Modules/Shaders/generated/cpp )
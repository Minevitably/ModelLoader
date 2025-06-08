cmake_minimum_required(VERSION 3.26)
project(ModelLoader)

set(CMAKE_CXX_STANDARD 17)

# 使用 vcpkg 的 CONFIG 模式查找依赖库
find_package(glad CONFIG REQUIRED)
find_package(glfw3 CONFIG REQUIRED)
find_package(assimp CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)
find_package(Threads REQUIRED)
# stb 是 header-only，不需要 find_package

# 编译可执行文件
add_executable(ModelLoader
        src/main.cpp
        src/stb_image.cpp
)


# 链接依赖库
target_link_libraries(ModelLoader PRIVATE
        glad::glad
        glfw
        assimp::assimp
        glm::glm
        Threads::Threads
)

# 添加一个名为 CopyResources 的目标，始终运行拷贝命令
add_custom_target(CopyResources ALL
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/resources/
        ${CMAKE_BINARY_DIR}/resources/
        COMMENT "Copying resources/images to build/resources/"
)

# 确保主程序构建时依赖资源拷贝
add_dependencies(ModelLoader CopyResources)
include_directories(${CMAKE_SOURCE_DIR}/includes)

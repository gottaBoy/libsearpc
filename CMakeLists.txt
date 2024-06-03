cmake_minimum_required(VERSION 3.10)
project(libsearpc VERSION 3.1.0 LANGUAGES C)

# 查找 pkg-config 模块
find_package(PkgConfig REQUIRED)

# 使用 pkg-config 查找 GLib 和 Jansson 库
pkg_check_modules(GLIB REQUIRED gobject-2.0>=2.26.0)
pkg_check_modules(JANSSON REQUIRED jansson>=2.2.1)

# 包含目录
include_directories(
    ${PROJECT_SOURCE_DIR}/include
    ${GLIB_INCLUDE_DIRS}
    ${JANSSON_INCLUDE_DIRS}
)

# 条件选项
option(ENABLE_COMPILE_DEMO "Compile demo programs" ON)

# 检测操作系统
if (WIN32)
    set(LIB_WS32 ws2_32)
    add_definitions(-DWIN32)
elseif (APPLE)
    add_definitions(-DMACOS)
elseif (CMAKE_SYSTEM_NAME STREQUAL "FreeBSD")
    add_definitions(-DFBSD)
endif()

# 添加子目录
add_subdirectory(lib)
if (ENABLE_COMPILE_DEMO)
    add_subdirectory(demo)
endif()
# add_subdirectory(tests)
add_subdirectory(pysearpc)

# 安装配置文件
configure_file(libsearpc.pc.in libsearpc.pc @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/libsearpc.pc DESTINATION lib/pkgconfig)

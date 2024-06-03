# pysearpc/CMakeLists.txt
# 假设你有一些 Python 脚本需要安装

# install(FILES searpc-codegen.py DESTINATION bin)
cmake_minimum_required(VERSION 3.10)
project(pysearpc_install)

# 定义 Python 文件
set(pysearpc_PYTHON
    __init__.py
    client.py
    common.py
    errors.py
    named_pipe.py
    server.py
    transport.py
    utils.py
)

# 获取 Python 安装目录
execute_process(
    COMMAND ${PYTHON_EXECUTABLE} -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())"
    OUTPUT_VARIABLE PYTHON_SITE_PACKAGES
    OUTPUT_STRIP_TRAILING_WHITESPACE
)

# 设置安装路径
set(pysearpcdir ${PYTHON_SITE_PACKAGES}/pysearpc)

# 安装 Python 文件
install(FILES ${pysearpc_PYTHON} DESTINATION ${pysearpcdir})


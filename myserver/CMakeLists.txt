file(GLOB c_sources *.c)
set_source_files_properties(${c_sources} PROPERTIES
  COMPILE_FLAGS "${WARNCFLAGS}")
file(GLOB cxx_sources *.cc)
set_source_files_properties(${cxx_sources} PROPERTIES
  COMPILE_FLAGS "${WARNCXXFLAGS} ${CXX1XCXXFLAGS}")

#include_directories(
#  ${CMAKE_CURRENT_SOURCE_DIR}
#  "${CMAKE_CURRENT_SOURCE_DIR}/../third-party"
#  "${CMAKE_CURRENT_SOURCE_DIR}/../third-party/llhttp/include"
#
#  ${LIBEVENT_INCLUDE_DIRS}
#  ${OPENSSL_INCLUDE_DIRS}
#)
# 要在CMake 中使用 `target_link_libraries` 来链接 `libevent` 库，你需要做以下几步：
#
#1. **找到 libevent 库的位置**:
#   首先，确保你知道 libevent 库的位置。这通常包括库文件（`.a` 或 `.so` 文件）以及头文件（`.h` 文件）的路径。
#
#2. **告诉 CMake 在哪里找到 libevent**:
#   在你的 `CMakeLists.txt` 文件中，使用 `find_package` 或者 `find_library` 命令告诉 CMake 在哪里找到 libevent。例如：
#
#   ```cmake
#   find_library(LIBEVENT_LIBRARY NAMES event PATHS /path/to/libevent/lib)
#   find_path(LIBEVENT_INCLUDE_DIR NAMES event.h PATHS /path/to/libevent/include)
#   ```
#
#3. **将 libevent 链接到你的目标**:
#   在 `target_link_libraries` 中添加 libevent 库。例如：
#
#   ```cmake
#   target_link_libraries(libevent-server ${LIBEVENT_LIBRARY})
#   ```
#
#4. **包含 libevent 的头文件**:
#   最后，确保你的源文件包含 libevent 的头文件。可以在 CMake 中使用 `target_include_directories` 来包含这些头文件。例如：
#
#   ```cmake
#   target_include_directories(libevent-server PUBLIC ${LIBEVENT_INCLUDE_DIR})
#   ```
#
#整合在一起，可能看起来像下面这样：
#
#```cmake
#find_library(LIBEVENT_LIBRARY NAMES event PATHS /path/to/libevent/lib)
#find_path(LIBEVENT_INCLUDE_DIR NAMES event.h PATHS /path/to/libevent/include)
#
#target_link_libraries(libevent-server ${LIBEVENT_LIBRARY})
#target_include_directories(libevent-server PUBLIC ${LIBEVENT_INCLUDE_DIR})
#```
#
#确保将路径替换为 libevent 库的实际路径。这样，CMake 将能够找到并链接 libevent 库到你的 `libevent-server` 目标中。
# 在软件开发中，"link_library"和"add_library"通常是用于构建系统（如CMake）中的命令，用于管理项目中的库。
#
#**link_library:**
#- `link_library` 是一个用于将库链接到目标（可执行文件或另一个库）的命令。它告诉链接器在构建过程中将指定的库链接到目标中。
#- 通常用于链接外部库或系统库，以便在构建可执行文件时将这些库包含到最终的可执行文件中。
#- 该命令通常用于指定第三方库，比如链接到OpenGL、Boost 等外部库。
#
#**add_library:**
#- `add_library` 是一个用于向项目中添加库的命令。它告诉构建系统如何构建库，并使得其他目标（如可执行文件或其他库）可以链接到这个库。
#- 通常用于将项目内部的代码组织成库，以便在项目中的不同部分共享代码。
#- 该命令用于创建并构建一个库，使得其他目标可以链接到这个库以使用其中定义的函数和类。
#
#**举例说明：**
#
#假设有一个项目，其中包含一个库和一个可执行文件。其中库包含一些通用函数，而可执行文件使用这些函数来完成特定任务。
#
#```cmake
## CMakeLists.txt
#
## 添加一个库，包含通用函数
#add_library(utils utils.cpp)
#
## 添加一个可执行文件，链接到库
#add_executable(main main.cpp)
#target_link_libraries(main utils)
#```
#
#在这个示例中，`add_library` 用于创建一个名为 `utils` 的库，它包含在文件 `utils.cpp` 中定义的通用函数。然后，`add_executable` 用于创建名为 `main` 的可执行文件，该可执行文件使用 `utils` 库中的函数。最后，`target_link_libraries` 命令将 `main` 可执行文件链接到 `utils` 库，以便在构建过程中将这些通用函数链接到 `main` 可执行文件中。
link_libraries(
  nghttp2
#  ${LIBEVENT_OPENSSL_LIBRARIES}
#  ${OPENSSL_LIBRARIES}
#  ${APP_LIBRARIES}
)
include_directories(/usr/local/include)
include_directories(/usr/local/include/event2)


# 在CMakeLists.txt文件中，`$<TARGET_OBJECTS:XXX>`是CMake的生成器表达式(generator expression)之一，用于将指定目标（target）中的对象文件（object files）添加到另一个目标中。这在构建项目时可以很有用，因为它允许将一个目标（target）中的编译对象（object files）直接包含到另一个目标中，而无需重复编译。
#
#举例说明如下：
#
#假设有两个CMake目标（target）：`target1`和`target2`，其中`target1`生成了一些对象文件，我们希望将这些对象文件包含到`target2`中。
#
#```cmake
#add_library(target1 OBJECT source1.cpp source2.cpp)
#add_executable(target2 main.cpp $<TARGET_OBJECTS:target1>)
#```
#
#在上面的例子中，`add_library()` 创建了一个名为`target1`的对象库，其中包含了`source1.cpp`和`source2.cpp`的编译对象。然后，`add_executable()` 创建了一个名为`target2`的可执行文件目标，使用了`$<TARGET_OBJECTS:target1>`表达式，以便将`target1`中的对象文件直接包含到`target2`的构建中。
#
#在你的例子中，假设有名为`llhttp`和`url-parser`的目标，通过`$<TARGET_OBJECTS:llhttp>`和`$<TARGET_OBJECTS:url-parser>`表达式，将这些目标生成的对象文件包含到`client`可执行文件的构建中。

# apt install libssl-dev libcrypto++-dev
add_executable(client           client.c $<TARGET_OBJECTS:llhttp>
  $<TARGET_OBJECTS:url-parser>
)
add_executable(libevent-client  libevent-client.c $<TARGET_OBJECTS:llhttp>
  $<TARGET_OBJECTS:url-parser>
)
#add_executable(libevent-server  libevent-server.c $<TARGET_OBJECTS:llhttp>
#  $<TARGET_OBJECTS:url-parser>
#)
add_executable(libevent-server  libevent-server.c)
target_link_libraries(libevent-server PUBLIC event)
# The error "undefined reference to `bufferevent_openssl_socket_new'" typically occurs when the linker is unable to find the definition of the function `bufferevent_openssl_socket_new`.
#
#To resolve this issue, you need to ensure that you are linking against the correct library that contains the definition of `bufferevent_openssl_socket_new`. In this case, it seems like you are using libevent with OpenSSL.
#
#You can fix this error by ensuring that you are linking against the necessary libraries. When compiling your program, make sure to include the appropriate flags to link against libevent and OpenSSL libraries. For example, you might need to include flags like `-levent`, `-levent_openssl`, and `-lssl`, depending on your setup.
#
#Here is an example of how you can compile a program that uses libevent with OpenSSL:
#
#```bash
#gcc your_program.c -o your_program -levent -levent_openssl -lssl -lcrypto
#```
#
#Make sure that the paths to the libraries are correctly set in case they are not in the default library path.
#
#After making these changes, try compiling your program again. This should resolve the "undefined reference" error related to `bufferevent_openssl_socket_new`.
target_link_libraries(libevent-server PUBLIC event_openssl)
#target_link_libraries(libevent-server PUBLIC nghttp2)
target_link_libraries(libevent-server PUBLIC ssl)
# /usr/local/lib/libcrypto.so.1.1: error adding symbols: DSO missing from command line
target_link_libraries(libevent-server PUBLIC crypto)
# /data/code_src/nghttp2/cmake-build-debug/myserver/libevent-server: error while loading shared libraries: libcrypto.so.1.1: cannot open shared object file: No such file or directory
# ln -s /usr/lib/x86_64-linux-gnu/libssl.so.3 /usr/lib/libssl.so.1.1
# ln -s /usr/lib/x86_64-linux-gnu/libcrypto.so.3 /usr/lib/libcrypto.so.1.1
# openssl 1.1.0 installation https://www.openssl.org/source/openssl-1.1.0.tar.gz
add_executable(deflate          deflate.c $<TARGET_OBJECTS:llhttp>
  $<TARGET_OBJECTS:url-parser>
)

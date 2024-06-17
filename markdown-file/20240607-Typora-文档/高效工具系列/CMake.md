[toc]



# 常用用法



## 1、预定义宏

### 总结：

```
PROJECT_SOURCE_DIR
```



### 讲解：

CMake 提供了一些预定义的宏（变量），这些宏在项目配置和生成过程中提供有用的信息。以下是一些常用的预定义宏及其用途：

#### CMake版本信息

- **`CMAKE_MAJOR_VERSION`**: CMake 的主版本号。
- **`CMAKE_MINOR_VERSION`**: CMake 的次版本号。
- **`CMAKE_PATCH_VERSION`**: CMake 的补丁版本号。
- **`CMAKE_VERSION`**: CMake 的完整版本号（格式为 `major.minor.patch`）。

```cmake
message(STATUS "CMake version: ${CMAKE_VERSION}")
```

#### 系统信息

- **`CMAKE_SYSTEM`**: 操作系统的名称，格式为 `<OS>-<CPU>`。
- **`CMAKE_SYSTEM_NAME`**: 操作系统的名称，如 `Linux`、`Windows`、`Darwin`。
- **`CMAKE_SYSTEM_VERSION`**: 操作系统的版本号。
- **`CMAKE_SYSTEM_PROCESSOR`**: 处理器名称，如 `x86_64`、`armv7l`。

```cmake
message(STATUS "System: ${CMAKE_SYSTEM_NAME} ${CMAKE_SYSTEM_VERSION}")
message(STATUS "Processor: ${CMAKE_SYSTEM_PROCESSOR}")
```

#### 项目信息

- **`CMAKE_PROJECT_NAME`**: 最顶层的 CMake 项目名称（即 `project()` 指令指定的名称）。
- **`CMAKE_PROJECT_VERSION`**: 项目的完整版本号（如果使用 `project(VERSION)` 指定）。
- **`CMAKE_PROJECT_VERSION_MAJOR`**: 项目的主版本号。
- **`CMAKE_PROJECT_VERSION_MINOR`**: 项目的次版本号。
- **`CMAKE_PROJECT_VERSION_PATCH`**: 项目的补丁版本号。

```cmake
project(MyProject VERSION 1.2.3)
message(STATUS "Project name: ${CMAKE_PROJECT_NAME}")
message(STATUS "Project version: ${CMAKE_PROJECT_VERSION}")
```

#### 编译器信息

- **`CMAKE_C_COMPILER`**: C 编译器的路径。
- **`CMAKE_CXX_COMPILER`**: C++ 编译器的路径。
- **`CMAKE_C_COMPILER_ID`**: C 编译器的标识符，如 `GNU`、`Clang`、`MSVC`。
- **`CMAKE_CXX_COMPILER_ID`**: C++ 编译器的标识符。

```cmake
message(STATUS "C Compiler: ${CMAKE_C_COMPILER} (${CMAKE_C_COMPILER_ID})")
message(STATUS "C++ Compiler: ${CMAKE_CXX_COMPILER} (${CMAKE_CXX_COMPILER_ID})")
```

#### 构建目录信息

- **`CMAKE_SOURCE_DIR`**: 最顶层 CMakeLists.txt 所在的目录。
- **`CMAKE_BINARY_DIR`**: 运行 CMake 时生成的构建目录。
- **`CMAKE_CURRENT_SOURCE_DIR`**: 当前处理的 CMakeLists.txt 所在的目录。
- **`CMAKE_CURRENT_BINARY_DIR`**: 当前处理的构建目录。

```cmake
message(STATUS "Source dir: ${CMAKE_SOURCE_DIR}")
message(STATUS "Binary dir: ${CMAKE_BINARY_DIR}")
```

#### 其他有用的宏

- **`CMAKE_BUILD_TYPE`**: 当前的构建类型（如 `Debug`、`Release`）。
- **`CMAKE_INSTALL_PREFIX`**: 安装前缀（默认是 `/usr/local`）。
- **`CMAKE_MODULE_PATH`**: 用于搜索模块文件的路径列表。
- **`CMAKE_PREFIX_PATH`**: 用于搜索软件包的路径前缀列表。

```cmake
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "Install prefix: ${CMAKE_INSTALL_PREFIX}")
```

#### 检查变量是否定义

- **`DEFINED`**: 检查一个变量是否已定义。

```cmake
if(DEFINED SOME_VARIABLE)
    message(STATUS "SOME_VARIABLE is defined")
else()
    message(STATUS "SOME_VARIABLE is not defined")
endif()
```

#### 示例

下面是一个示例 CMakeLists.txt，展示了如何使用这些预定义宏：

```cmake
cmake_minimum_required(VERSION 3.10)

project(MyProject VERSION 1.2.3)

message(STATUS "CMake version: ${CMAKE_VERSION}")
message(STATUS "System: ${CMAKE_SYSTEM_NAME} ${CMAKE_SYSTEM_VERSION}")
message(STATUS "Processor: ${CMAKE_SYSTEM_PROCESSOR}")
message(STATUS "Project name: ${CMAKE_PROJECT_NAME}")
message(STATUS "Project version: ${CMAKE_PROJECT_VERSION}")
message(STATUS "C Compiler: ${CMAKE_C_COMPILER} (${CMAKE_C_COMPILER_ID})")
message(STATUS "C++ Compiler: ${CMAKE_CXX_COMPILER} (${CMAKE_CXX_COMPILER_ID})")
message(STATUS "Source dir: ${CMAKE_SOURCE_DIR}")
message(STATUS "Binary dir: ${CMAKE_BINARY_DIR}")
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "Install prefix: ${CMAKE_INSTALL_PREFIX}")

if(DEFINED SOME_VARIABLE)
    message(STATUS "SOME_VARIABLE is defined")
else()
    message(STATUS "SOME_VARIABLE is not defined")
endif()
```

这个示例会输出CMake的版本、系统信息、项目名称和版本、编译器信息、源代码和构建目录路径、构建类型以及安装前缀。如果定义了 `SOME_VARIABLE`，还会输出其状态。通过这些预定义宏，可以更方便地调试和配置CMake项目。



## 2、message

### 总结：

```c
message(STATUS "PROJECT_SOURCE_DIR: " ${PROJECT_SOURCE_DIR})
```



### 讲解：

`CMake`中的`message()`命令用于在配置和生成过程的不同阶段向用户输出消息。它可以用于调试、状态报告或输出错误和警告。以下是`message()`命令的用法和常见选项：

#### 基本语法

```cmake
message(<mode> "message to display")
```

#### 模式选项

1. **STATUS**: 输出一般信息。默认情况下，这些消息会显示在终端中。
   ```cmake
   message(STATUS "This is a status message.")
   ```

2. **WARNING**: 输出警告消息。这些消息表示一个潜在的问题，但不会停止配置过程。
   ```cmake
   message(WARNING "This is a warning message.")
   ```

3. **AUTHOR_WARNING**: 输出作者级别的警告消息。这些消息是专门给项目维护者和作者看的。
   
   ```cmake
   message(AUTHOR_WARNING "This is an author warning message.")
   ```
   
4. **SEND_ERROR**: 输出错误消息，并停止配置过程。生成阶段将不会继续。
   ```cmake
   message(SEND_ERROR "This is an error message.")
   ```

5. **FATAL_ERROR**: 输出致命错误消息，并立即停止CMake过程。这会立即中止配置和生成。
   ```cmake
   message(FATAL_ERROR "This is a fatal error message.")
   ```

6. **DEPRECATION**: 输出弃用警告消息，通常用于表示某个功能或命令在未来版本中将不再支持。
   
   ```cmake
   message(DEPRECATION "This is a deprecation warning.")
   ```

#### 例子

#### 打印状态消息

```cmake
message(STATUS "Starting the build process")
```

#### 打印变量值（调试）

```cmake
set(MY_VAR "Hello, CMake!")
message(STATUS "MY_VAR is set to ${MY_VAR}")
```

#### 打印警告消息

```cmake
message(WARNING "This feature is experimental and might not work as expected.")
```

#### 打印错误消息并停止配置

```cmake
if(NOT DEFINED SOME_REQUIRED_VARIABLE)
    message(FATAL_ERROR "SOME_REQUIRED_VARIABLE is not set. Please define it before running CMake.")
endif()
```

#### 打印弃用警告

```cmake
message(DEPRECATION "This function is deprecated and will be removed in future releases.")
```

### 高级用法

#### 控制消息的显示条件

你可以使用条件语句来控制消息的显示。例如，只在调试模式下显示消息：

```cmake
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    message(STATUS "This is a debug build.")
endif()
```

#### 多行消息

CMake的`message()`命令也支持多行消息输出：

```cmake
message(STATUS "This is a multi-line message.
Line 1
Line 2
Line 3")
```

### 总结

`message()`命令是CMake中一个非常有用的工具，它不仅可以帮助调试和记录信息，还可以在构建过程中向用户传达重要的状态、警告和错误消息。通过适当的使用，可以大大提高构建脚本的可读性和可维护性。





## 3、如何获取头文件和源代码文件

在CMake中，可以使用一些变量和命令来获取项目中的头文件和源代码文件。这些文件通常会存放在变量中，然后传递给 `target_include_directories`、`target_sources` 或 `add_executable`/`add_library` 命令。

以下是一个示例，展示如何使用CMake获取头文件和源代码文件：

### 示例项目结构

假设你的项目结构如下：

```
MyProject/
├── CMakeLists.txt
├── include/
│   ├── MyProject/
│   │   ├── header1.h
│   │   └── header2.h
├── src/
│   ├── main.cpp
│   ├── source1.cpp
│   └── source2.cpp
└── tests/
    ├── test1.cpp
    └── test2.cpp
```

### CMakeLists.txt 内容

```cmake
cmake_minimum_required(VERSION 3.10)

project(MyProject VERSION 1.0)

# 设置C++标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 获取头文件
file(GLOB_RECURSE HEADER_FILES ${CMAKE_SOURCE_DIR}/include/MyProject/*.h)

# 获取源代码文件
file(GLOB_RECURSE SOURCE_FILES ${CMAKE_SOURCE_DIR}/src/*.cpp)

# 打印头文件和源代码文件
message(STATUS "Header files: ${HEADER_FILES}")
message(STATUS "Source files: ${SOURCE_FILES}")

# 添加可执行文件
add_executable(MyProject ${SOURCE_FILES} ${HEADER_FILES})

# 包含头文件目录
target_include_directories(MyProject PRIVATE ${CMAKE_SOURCE_DIR}/include)
```

### 解释

1. **项目配置**:
   
   ```cmake
   cmake_minimum_required(VERSION 3.10)
   project(MyProject VERSION 1.0)
   ```
   
2. **设置C++标准**:
   ```cmake
   set(CMAKE_CXX_STANDARD 11)
   set(CMAKE_CXX_STANDARD_REQUIRED True)
   ```

3. **获取头文件**:
   ```cmake
   file(GLOB_RECURSE HEADER_FILES ${CMAKE_SOURCE_DIR}/include/MyProject/*.h)
   ```
   `file(GLOB_RECURSE ...)` 命令会递归地获取 `include/MyProject/` 目录下的所有 `.h` 文件，并将它们存储在 `HEADER_FILES` 变量中。

4. **获取源代码文件**:
   ```cmake
   file(GLOB_RECURSE SOURCE_FILES ${CMAKE_SOURCE_DIR}/src/*.cpp)
   ```
   `file(GLOB_RECURSE ...)` 命令会递归地获取 `src/` 目录下的所有 `.cpp` 文件，并将它们存储在 `SOURCE_FILES` 变量中。

5. **打印头文件和源代码文件**:
   ```cmake
   message(STATUS "Header files: ${HEADER_FILES}")
   message(STATUS "Source files: ${SOURCE_FILES}")
   ```
   使用 `message()` 命令来输出这些文件列表，以便调试和验证。

6. **添加可执行文件**:
   ```cmake
   add_executable(MyProject ${SOURCE_FILES} ${HEADER_FILES})
   ```
   将获取到的源文件和头文件传递给 `add_executable()` 命令，创建一个名为 `MyProject` 的可执行文件。

7. **包含头文件目录**:
   ```cmake
   target_include_directories(MyProject PRIVATE ${CMAKE_SOURCE_DIR}/include)
   ```
   使用 `target_include_directories()` 命令将 `include` 目录添加到编译器的头文件搜索路径中。

### 其他注意事项

- **使用 `file(GLOB ...)`**:
  - `file(GLOB ...)` 是一种方便的方式来获取文件列表，但并不是动态的。如果你添加或删除文件，需要重新运行 CMake。
  - 对于更复杂的项目，建议手动维护文件列表，以确保CMake配置文件的可读性和可维护性。

- **手动维护文件列表**:
  - 你可以手动列出所有源文件和头文件，而不是使用 `file(GLOB ...)`。**这在大型项目中尤其常见**，因为它使文件变动更加明确。

```cmake
set(HEADER_FILES
    ${CMAKE_SOURCE_DIR}/include/MyProject/header1.h
    ${CMAKE_SOURCE_DIR}/include/MyProject/header2.h
)

set(SOURCE_FILES
    ${CMAKE_SOURCE_DIR}/src/main.cpp
    ${CMAKE_SOURCE_DIR}/src/source1.cpp
    ${CMAKE_SOURCE_DIR}/src/source2.cpp
)

add_executable(MyProject ${SOURCE_FILES} ${HEADER_FILES})
```

通过上述方式，你可以有效地管理你的头文件和源代码文件，并确保CMake能够正确找到和使用它们。





# Others

## 1、file(GLOB 和file(GLOB_RECURSE有什么区别

`file(GLOB ...)` 和 `file(GLOB_RECURSE ...)` 是 CMake 中用于获取文件列表的两个命令，它们之间的主要区别在于是否递归地搜索子目录。

### `file(GLOB ...)`

- **功能**: 获取匹配特定模式的文件列表，不递归搜索子目录。
- **用法**: 
  ```cmake
  file(GLOB <variable> [RELATIVE <path>] [globbing expressions]...)
  ```
- **参数**:
  - `<variable>`: 存储结果的变量。
  - `[RELATIVE <path>]`: 如果指定，这个路径将作为结果列表中每个文件的相对路径的基准。
  - `[globbing expressions]...`: 匹配文件的模式（如 `*.cpp`、`*.h`）。
  
- **示例**:
  ```cmake
  file(GLOB SOURCE_FILES ${CMAKE_SOURCE_DIR}/src/*.cpp)
  ```
  这个命令将获取 `src` 目录下所有 `.cpp` 文件，但不包括子目录中的文件。

### `file(GLOB_RECURSE ...)`

- **功能**: 递归地获取匹配特定模式的文件列表，包括所有子目录。
- **用法**:
  ```cmake
  file(GLOB_RECURSE <variable> [RELATIVE <path>] [FOLLOW_SYMLINKS] [globbing expressions]...)
  ```
- **参数**:
  - `<variable>`: 存储结果的变量。
  - `[RELATIVE <path>]`: 如果指定，这个路径将作为结果列表中每个文件的相对路径的基准。
  - `[FOLLOW_SYMLINKS]`: 如果指定，将跟随符号链接。
  - `[globbing expressions]...`: 匹配文件的模式（如 `*.cpp`、`*.h`）。

- **示例**:
  ```cmake
  file(GLOB_RECURSE SOURCE_FILES ${CMAKE_SOURCE_DIR}/src/*.cpp)
  ```
  这个命令将获取 `src` 目录及其所有子目录中的所有 `.cpp` 文件。

### 使用示例

假设项目目录结构如下：

```
MyProject/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   ├── utils/
│   │   └── helper.cpp
│   └── components/
│       └── component.cpp
```

#### 使用 `file(GLOB ...)`

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject)

# 获取 src 目录下的所有 .cpp 文件，不包括子目录中的文件
file(GLOB SOURCE_FILES ${CMAKE_SOURCE_DIR}/src/*.cpp)

# 打印获取到的文件列表
message(STATUS "Source files: ${SOURCE_FILES}")

add_executable(MyProject ${SOURCE_FILES})
```

输出可能为：
```
-- Source files: /path/to/MyProject/src/main.cpp
```

#### 使用 `file(GLOB_RECURSE ...)`

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject)

# 递归获取 src 目录及其子目录中的所有 .cpp 文件
file(GLOB_RECURSE SOURCE_FILES ${CMAKE_SOURCE_DIR}/src/*.cpp)

# 打印获取到的文件列表
message(STATUS "Source files: ${SOURCE_FILES}")

add_executable(MyProject ${SOURCE_FILES})
```

输出可能为：
```
-- Source files: /path/to/MyProject/src/main.cpp;/path/to/MyProject/src/utils/helper.cpp;/path/to/MyProject/src/components/component.cpp
```

### 选择 `file(GLOB ...)` 还是 `file(GLOB_RECURSE ...)`

- **使用 `file(GLOB ...)`**:
  - 当你只需要获取某个特定目录中的文件，而不需要递归搜索子目录时。
  - 适用于文件层次结构较简单的项目。
  
- **使用 `file(GLOB_RECURSE ...)`**:
  - 当你需要递归地获取目录及其所有子目录中的文件时。
  - 适用于文件层次结构较复杂的项目，或者你希望自动包含所有子目录中的文件。

### 注意事项

- `file(GLOB ...)` 和 `file(GLOB_RECURSE ...)` 获取的文件列表不会动态更新。如果添加或删除了文件，需要重新运行 CMake 来更新文件列表。
- 在大型项目中，手动维护文件列表可能会更加清晰和可控。使用 `file(GLOB ...)` 和 `file(GLOB_RECURSE ...)` 的自动化方式虽然方便，但可能会使项目结构的变动不够显式。

通过了解 `file(GLOB ...)` 和 `file(GLOB_RECURSE ...)` 的区别及其使用场景，可以更有效地管理和配置 CMake 项目中的文件。
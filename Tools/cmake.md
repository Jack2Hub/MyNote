




cmake_minimum_required(VERSION 3.5.1)

PROJECT(UDP2)


set(CMAKE_C_COMPILER /opt/gcc-arm-8.2-2018.08-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc)
set(CMAKE_CXX_COMPILER /opt/gcc-arm-8.2-2018.08-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-g++)

#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -o2 -fPIC")


# head file
include_directories(./)

# lib path
# link_directories(./)

aux_source_directory(./ SRCS_DIR)

add_library(lowlatency_protocol ${SRCS_DIR})

target_link_libraries(lowlatency_protocol pthread)

# install
## 关键字 PROJECT_SOURCE_DIR
> cmake 工程所在目录

### install(TARGETS)
> 参数中的TARGETS后面跟的就是我们通过ADD_EXECUTABLE或者ADD_LIBRARY定义的目标文件，可能是可执行二进制、动态库、静态库。目标类型也就相对应的有三种，ARCHIVE特指静态库，LIBRARY特指动态库，RUNTIME特指可执行目标二进制。
```
set(CMAKE_INSTALL_PREFIX ${PROJECT_SOURCE_DIR}/build)
install(TARGETS lowlatency_protocol
		ARCHIVE DESTINATION install)
install(FILES ${PROJECT_SOURCE_DIR}/MindNVR.h DESTINATION install)
install(FILES ${PROJECT_SOURCE_DIR}/package.h DESTINATION install)
```
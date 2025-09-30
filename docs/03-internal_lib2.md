In this lab we will go thru the process of adding a second library call it `blinker`

[ansible@ansible cmake_tutorial]$ `ls`

`build  CMakeLists.txt  main.c`

Create the libraray source and header directories

[ansible@ansible cmake_tutorial]$ `mkdir -p lib/libblinker/{include,src}`

Create the header file with the header guard

[ansible@ansible cmake_tutorial]$ `vim lib/libblinker/include/display.h`
```
#ifndef BLINKER_H
#define BLINKER_H

void blink(char *c);

#endif
```

Create the library source code

[ansible@ansible cmake_tutorial]$ `vim lib/libblinker/src/blink.c`
```
#include <stdio.h>

void blink(char *c)
{
        printf("%s", c);
}
```

Create the libraries `CMakeLists.txt` configuration, pointing to the library and the header files(there are more details in the options that chosen with, such as having the library as static or shared, and the scope which could be public or private). The library name/(target name) here is displayer.

[ansible@ansible cmake_tutorial]$ `vim lib/CMakeLists.txt`

```
add_library(displayer STATIC ${CMAKE_CURRENT_SOURCE_DIR}/libdisplayer/src/display.c)
add_library(blinker STATIC ${CMAKE_CURRENT_SOURCE_DIR}/libblinker/src/blink.c)

target_include_directories(displayer PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/libdisplayer/include)
target_include_directories(blinker PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/libblinker/include)
```

Update the main `CMakeLists.txt` to point to the new added library and link header

[ansible@ansible cmake_tutorial]$ `vim CMakeLists.txt`
```
cmake_minimum_required(VERSION 3.20)

project(main VERSION 1.0.0 LANGUAGES C CXX)

add_executable(${PROJECT_NAME} main.c)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/lib)

target_link_libraries(${PROJECT_NAME} PRIVATE displayer blinker)
```

Switch to build directory and build

`cd build`

`cmake ..`

`make`




Prev: [First cmake application](01-First_cmake_application.md)                                                                                         Next: [Add external library II](03-internal_lib2.md)






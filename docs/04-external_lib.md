In order to add extenal library we will need to install the library `-devel` pkg (e.g. `SDL2-devel`), add it with `find_package`, then link it in the CMakeLists.txt

[ansible@ansible cmake_tutorial]$ `sudo dnf install SDL2-devel -y`

Update `CMakeLists.txt` to reflect the external library

[ansible@ansible cmake_tutorial]$ `vim CMakeLists.txt`
```cmake_minimum_required(VERSION 3.20)

project(main VERSION 1.0.0 LANGUAGES C CXX)

add_executable(${PROJECT_NAME} main.c)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/lib)

find_package(SDL2 REQUIRED)
if (SDL2_FOUND)
        target_link_libraries(${PROJECT_NAME} PRIVATE displayer SDL2::SDL2)
endif()

target_link_libraries(${PROJECT_NAME} PRIVATE displayer)
```

Update `main.c` by adding SDL header and some SDL initialization, such as below

[ansible@ansible cmake_tutorial]$ `vim main.c`
```
#include <stdio.h>
#include <display.h>
#include <SDL.h>

int main()
{
        display("A one file c program\n");

        if(SDL_Init(SDL_INIT_VIDEO) < 0)
        {
                printf("SDL could not be initialized!\n"
                                "SDL_Error: %s\n", SDL_GetError());
                return 0;
        }
        return 0;
}
```


Prev: [Add internal library II](03-internal_lib2.md)                                                                                         Next: [Add installation parameters](05-Add_install_param.md)                                                                               

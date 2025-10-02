Now we developed our application and we need to deploy it in a directory according to our company standards. So, I tried to trim as much as possible of the information and code to keep the configuration and the landing to the cmake installation process as soft as possible.

- Below is the project cmake project structure, as I needed to have update in order to have clean installation structure.

`tree make_tutorial/`
```
  ../cmake_tutorial/
  ├── CMakeLists.txt
  ├── include
  │   ├── blink.h
  │   └── display.h
  ├── lib
  │   ├── blink.c
  │   ├── CMakeLists.txt
  │   └── display.c
  └── main.c
```

- This is the cmake in the root directory, which has the **install** instruction inthe last line, this will copy the **main** executable to the **bin** directory

`cat CMakeLists.txt`

  ```
  cmake_minimum_required(VERSION 3.20)
  
  project(main VERSION 1.0.0 LANGUAGES C CXX)
  
  add_executable(${PROJECT_NAME} main.c)
  
  add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/lib)
  
  find_package(SDL2 REQUIRED)
  if (SDL2_FOUND)
          target_link_libraries(${PROJECT_NAME} PRIVATE SDL2::SDL2)
  endif()
  
  target_link_libraries(${PROJECT_NAME} PRIVATE displayer blinker)
  
  install(TARGETS ${PROJECT_NAME})
  ```


Resources

https://www.youtube.com/watch?v=OMx3cZj_hoo

Prev: [Add External library](04-external_lib.md)                                                                                       

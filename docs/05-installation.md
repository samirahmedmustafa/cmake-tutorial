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

- This is the cmake in the root directory, which has the **install** instruction in the last line, this will copy the **main** executable to the **bin** directory

default behaviour:
  Executables → go to ${CMAKE_INSTALL_BINDIR} (default: bin)
  Libraries → go to ${CMAKE_INSTALL_LIBDIR} (default: lib or lib64)
  Headers → go to ${CMAKE_INSTALL_INCLUDEDIR} (default: include)

`cat CMakeLists.txt`

  ```
  cmake_minimum_required(VERSION 3.20)
  
  project(main VERSION 1.0.0 LANGUAGES C CXX)
  
  add_executable(${PROJECT_NAME} main.c)
  
  add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/lib)

  # This part is from the previous lab related to the external libraries
  find_package(SDL2 REQUIRED)
  if (SDL2_FOUND)
          target_link_libraries(${PROJECT_NAME} PRIVATE SDL2::SDL2)
  endif()
  
  target_link_libraries(${PROJECT_NAME} PRIVATE displayer blinker)
  
  install(TARGETS ${PROJECT_NAME}) #This what install the project executable to 
  ```
- I used SHARED library here as STATIC library will not demonstrate much(as executable wo't rely on libraries in the runtime), so basically, I used cmake variables here such as CMAKE_SOURCE_DIR which points to the project root directory. This instruction $<INSTALL_INTERFACE:include/> called generator expression which means if INSTALL_INTERFACE is true (cmake --install) then `include` will be relative to the PREFIX of the installation. The first `install` instruction will take the source directory `${CMAKE_SOURCE_DIR}/include` of headers, and `DESTINATION` with a `.` means current directory, `FILES_MATCHING PATTERN` to fetch all `*.h` headers. The last `install` is for propagating libraries to `lib` directory. 

`vim lib/CMakeLists.txt`

```
add_library(displayer SHARED ${CMAKE_SOURCE_DIR}/lib/display.c)
add_library(blinker SHARED ${CMAKE_SOURCE_DIR}/lib/blink.c)

target_include_directories(displayer PUBLIC
                                $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
                                $<INSTALL_INTERFACE:include/>                   #<prefix>/include/
                                )
target_include_directories(blinker PUBLIC
                                $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
                                $<INSTALL_INTERFACE:include/>                   #<prefix>/include/
                                )

install(
        DIRECTORY ${CMAKE_SOURCE_DIR}/include
        DESTINATION .
        FILES_MATCHING PATTERN "*.h"
)

install(TARGETS displayer)
install(TARGETS blinker)
```

- Previous we used `cmake ../` from the build directory, now we will extend the usage of cmake (configure, build, install) to be as below

`cmake -S . -B build/`                              # to configure

`cmake --build build/ -j12`                         # to build

`cmake --install build --prefix /product/myapp/`    # to install

- The end result of the application structure is as below

```
tree /product/myapp/
/product/myapp/
├── bin
│   └── main
├── include
│   ├── blink.h
│   └── display.h
└── lib
    ├── libblinker.so
    └── libdisplayer.so
```

- Now, you execute the application `/product/myapp/bin/main` and get the runtime below error (which is familiar to the system admins) `/product/myapp/bin/main: error while loading shared libraries: libdisplayer.so: cannot open shared object file: No such file or directory`. You can export `LD_LIBRARY_PATH` pointing to the libraries location such as below

`export LD_LIBRARY_PATH=/product/myapp/lib/:$LD_LIBRARY_PATH`

`/product/myapp/bin/main`

```
displayer:
Display line one file c program
blinker:
Blink line one file c program
```

Now, since you build the libraries along with the project, which are expected to be used as APIs for building other projects, think such as SDL-devel which got installed in the standard library path. So, can we use those libraries we just installed with `find_package` to build another cmake project? Let's do that in another lab, whenever it is feasible.

Resources

https://www.youtube.com/watch?v=OMx3cZj_hoo

Prev: [Add External library](04-external_lib.md)                                                                                       

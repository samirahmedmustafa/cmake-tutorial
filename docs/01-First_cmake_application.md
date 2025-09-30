In this lab, we will create one file c program compile it using gcc, then convert it into cmake

[ansible@ansible ~]$ `mkdir cmake_tutorial`

[ansible@ansible ~]$ `cd cmake_tutorial`

[ansible@ansible cmake_tutorial]$ `vim main.c`

```
#include <stdio.h>

int main()
{
        printf("A one file c program\n");
        return 0;
}
```

[ansible@ansible cmake_tutorial]$ `gcc main.c -o main`


[ansible@ansible cmake_tutorial]$ `./main`
```
A one file c program
```

Now, I will remove the executable `main`, then let us convert this application to cmake instead of using gcc

[ansible@ansible cmake_tutorial]$ `rm main`

Create the main cmake builder file, which will include the `minimum cmake version`, `(project name, version, type)`, and executable named `main` (the term used by cmake is target) along with the source code file `main.c` 

[ansible@ansible cmake_tutorial]$ `vim CMakeLists.txt`

```
cmake_minimum_required(VERSION 3.20)

project(main VERSION 1.0.0 LANGUAGES C CXX)

add_executable(main main.c)
```

We can start using embedded variables, change executable name/(target name) to ${PROJECT_NAME}, so it becomes

```
cmake_minimum_required(VERSION 3.20)

project(main VERSION 1.0.0 LANGUAGES C CXX)

add_executable(${PROJECT_NAME} main.c)
```

The standard on cmake is to create a directory `build`, then all built binaries goes to it

[ansible@ansible cmake_tutorial]$ `mkdir build`

[ansible@ansible cmake_tutorial]$ `cd build`

To generate the Makefile

[ansible@ansible build]$ `cmake ../`

To build the code

[ansible@ansible build]$ `cmake --build .` or `make`

To build the code with debuginfo symbols

[ansible@ansible build]$ `cmake -DCMAKE_BUILD_TYPE=Debug .`

Execute the code

[ansible@ansible build]$ `./main`
```
A one file c program
```

Previous: [Prerequisites](0-prerequisites.md) Next: [Add internal library I](02-internal_lib1.md)


Now we developed our application and we need to deploy it in a directory according to our company standards. So, I tried to trim as much as possible of the information and code to keep the configuration and the landing to the cmake installation process as soft as possible.

- Below is the project cmake project structure, as I needed to update to have clean installation structure.

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



Resources

https://www.youtube.com/watch?v=OMx3cZj_hoo

Prev: [Add External library](04-external_lib.md)                                                                                       

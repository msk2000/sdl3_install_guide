# SDL3 Manual Build and Install

The [official SDL3 installation guide](https://github.com/libsdl-org/SDL/blob/main/docs/INTRO-cmake.md) focuses on project-by-project setup (usually using SDL as a submodule or vendored dependency). While that works, I preferred a system-wide installation so that I could:

- Use SDL3 easily across multiple projects without repeating setup steps.
- Compile example code and tutorials from anywhere without extra configuration.
- Link against SDL3 manually or via CMake, just like I used to do with SDL2.

This guide documents the steps I followed to set up SDL3 system-wide on my Kubuntu 24.04 system.

## Remove SDL2 (if already installed)
Open your terminal and get rid of the previous SDL installations (the versions/files will be different based on what you actually have installed):

```bash
sudo apt remove --purge libsdl2-dev libsdl2-2.0-0
sudo apt autoremove
```

# Clone the SDL3 repo
Clone the official repo and pick the most stable version from there:
```bash
git clone https://github.com/libsdl-org/SDL.git
cd SDL
git checkout release-3.0.0  # optional stable version
```

# Create build directory
```bash
mkdir build && cd build
```

# Configure the build

Now we configure Cmake. You can see [the explanation](cmake_explanation.txt) for the following config if you want.
```bash
$ cmake .. -DCMAKE_BUILD_TYPE=Release -DSDL_TEST=OFF -DSDL_STATIC=OFF -DCMAKE_INSTALL_PREFIX=/usr/local
```

# Compile and install system-wide
```bash
make -j$(nproc)
sudo make install
sudo ldconfig
```

Now that SDL3 is installed system-wide, you can start experimenting with it right away.

You can follow tutorials or sample projects and compile them either:

- Directly from the terminal using g++ or clang++ with sdl3-config for flags:

```bash
g++ your_file.cpp $(sdl3-config --cflags --libs) -o your_program
```
- Or set up a CMake-based project and link against SDL3:
```cmake
find_package(SDL3 REQUIRED CONFIG)
target_link_libraries(your_target PRIVATE SDL3::SDL3)
```



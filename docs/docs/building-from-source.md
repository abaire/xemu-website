## Binaries

Users are recommended to use the [pre-built xemu binaries](https://github.com/mborgerson/xemu/wiki#download). If you would like to build from source however, follow the instructions for your platform below.

## Windows

Windows builds are cross-compiled from Ubuntu. If you would like to build *on* Windows, you can use WSL2 and Docker. See [official Docker
documentation](https://docs.docker.com/docker-for-windows/wsl/) for how to get WSL2 and Docker set up.

```bash
# Clone and build
git clone https://github.com/mborgerson/xemu.git
docker run --rm -v $PWD/xemu:/xemu -w /xemu \
    -e CCACHE_DIR=/xemu/ccache \
    mborgerson/xemu-ubuntu-win64-cross:latest \
    ./build.sh -p win64-cross

# Run
./xemu/dist/xemu.exe
```

### Debugging with Visual Studio

1. Install [MSYS2](https://www.msys2.org/)
1. Build xemu as above.
1. Open the xemu directory in Visual Studio as a "local folder"
1. In Solution Explorer, make sure the "Show All Files" option is enabled and right click on `xemu.exe` inside of the `dist` directory.
1. Select "Add debug configuration"
1. From the dialog, select `Launch for MinGW/Cygwin (gdb)`. An editor for `lauch.vs.json` should appear.
1. Add a target configuration. For example:
    ```
    "configurations": [
      {
        "type": "cppdbg",
        "name": "xemu.exe - test.iso",
        "project": "dist\\xemu.exe",
        "projectTarget": "",
        "cwd": "${workspaceRoot}",
        "program": "${debugInfo.target}",
        "args": [
          "-s",
          "-dvd_path",
          "X:\\xiso\\test.iso"
        ],
        "MIMode": "gdb",
        "miDebuggerPath": "${env.MINGW_PREFIX}\\bin\\gdb.exe",
        "externalConsole": true
      }
    ]

    ```
1. If you have not previously configured the MINGW_PREFIX environment, go to `Project` -> `Edit Settings` -> `CppProperties.json`
  1. In the resulting editor, add an `environments` configuration. For example:
      ```
      "environments": [
        {
          "MINGW_PREFIX": "C:/msys64/mingw64",
          "MINGW_CHOST ": "x86_64-w64-mingw32",
          "MINGW_PACKAGE_PREFIX": "mingw-w64-x86_64",
          "MSYSTEM": "MINGW64",
          "MSYSTEM_CARCH": "x64_64",
          "MSYSTEM_PREFIX": "${env.MINGW_PREFIX}",
          "SHELL": "${env.MINGW_PREFIX}/../usr/bin/bash",
          "TEMP": "${env.MINGW_PREFIX}/../tmp",
          "TMP": "${env.TEMP}",
          "PATH": "${env.MINGW_PREFIX}/bin;${env.MINGW_PREFIX}/../usr/local/bin;${env.MINGW_PREFIX}/../usr/bin;${env.PATH}",
          "INCLUDE": "project/lib/include;${env.MINGW_PREFIX}/mingw/include"
        }
      ]  
      ```


## macOS

First install the [Homebrew package manager](https://brew.sh/).

```bash
# Install dependencies
brew update
brew install coreutils pkg-config dylibbundler ninja

# Clone and build
git clone https://github.com/mborgerson/xemu.git
cd xemu
./build.sh

# Run
open ./dist/xemu.app
```

## Linux

=== "Debian/Ubuntu"

    ```bash
    # Install dependencies
    sudo apt update
    sudo apt install git build-essential libsdl2-dev libepoxy-dev libpixman-1-dev libgtk-3-dev libssl-dev libsamplerate0-dev libpcap-dev ninja-build

    # Clone and build
    git clone https://github.com/mborgerson/xemu.git
    cd xemu
    ./build.sh

    # Run
    ./dist/xemu
    ```

=== "Arch Linux"

    ```bash
    # Install dependencies
    sudo pacman -S --noconfirm git base-devel sdl2 libepoxy pixman gtk3 openssl libsamplerate libpcap ninja glu

    # Clone and build
    git clone https://github.com/mborgerson/xemu.git
    cd xemu
    ./build.sh

    # Run
    ./dist/xemu
    ```

!!! tip "Tip: Building with Clang, or another specific compiler"

    If you have multiple toolchains and would like to build with specific one,
    such as clang, you can specify the compiler and linker through environment
    variables when invoking build.sh as follows:

    ```bash
    CC=clang CXX=clang++ CC_LD=lld CXX_LD=lld AR=llvm-ar ./build.sh
    ```

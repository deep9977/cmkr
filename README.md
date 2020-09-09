# cmkr

A CMakeLists.txt generator from TOML. Still WIP.

## Building
cmkr requires a C++17 compiler, cmake >= 3.14 and git. It depends on toml11 by ToruNiina, which is added as a git submodule.
```
git clone https://github.com/moalyousef/cmkr
cd cmkr
git submodule update --init --recursive
cmake -S. -Bbin
cmake --build bin
```

## Usage
cmkr parses cmake.toml files (using toml11) at the project directory. A basic hello world format with the minimum required fields:
```toml
[cmake]
minimum = "3.14"

[project]
name = "app"
version = "0.1.0"

[[bin]]
name = "app"
type = "exe"
sources = ["src/main.cpp"]
```

This project's cmake.toml:
```toml
[cmake]
minimum = "3.14"

[project]
name = "cmkr"
version = "0.1.0"

[[bin]]
name = "cmkrlib"
type = "static"
sources = ["src/args.cpp", "src/gen.cpp", "src/help.cpp"]
include-dirs = ["vendor"]
features = ["cxx_std_17"]

[[bin]]
name = "cmkr"
type = "exe"
sources = ["src/main.cpp"]
link-libs = ["cmkrlib"]
```

Currently supported fields:
```toml
[cmake] # required for top-level project
minimum = "3.14" # required
cpp-flags = [] # optional
c-flags = [] # optional
link-flags = [] # optional
subdirs = [] # optional

[project] # required per project
name = "app" # required
version = "0.1.0" # required

[find] # optional, runs find_package, use "*" to ignore version
boost = "1.74.0" # optional

[fetch] # optional, runs fetchContent
toml11 = { git = "https://github.com/ToruNiina/toml11", git-tag = "v3.5.0" } # optional

[[bin]] # required, can define several binaries
name = "app" # required
type = "exe" # required (exe || shared || static)
sources = ["src/main.cpp"] # required
include-dirs = [] # optional
features = [] # optional
defines = [] # optional
link-libs = [] # optional 
```

The cmkr executable can be run from the command-line:
```
Usage: cmkr [arguments]
arguments:
    init     [exe|shared|static]    Starts a new project in the same directory.
    gen                             Generates CMakeLists.txt file.
    build    <extra cmake args>     Run cmake and build.
    help                            Show help.
    version                         Current cmkr version.
```
The build command invokes cmake and the default build-system on your platform, it also accepts extra cmake arguments:
```
cmkr build -GNinja -DCMAKE_TOOLCHAIN_FILE=/path/to/toolchain -DCMAKE_BUILD_TYPE=Release 
```

## Roadmap
- Support more fields.
- Support conditional cmake args somehow!
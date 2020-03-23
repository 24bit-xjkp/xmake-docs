
## Installation

#### via curl

```bash
bash <(curl -fsSL https://xmake.io/shget.text)
```

#### via wget

```bash
bash <(wget https://xmake.io/shget.text -O -)
```

#### via powershell

```powershell
Invoke-Expression (Invoke-Webrequest 'https://xmake.io/psget.text' -UseBasicParsing).Content
```

## Simple description

<img src="https://xmake.io/assets/img/index/showcode1.png" width="340px" />

## Package dependences

<img src="https://xmake.io/assets/img/index/add_require.png" width="600px" />

An official xmake package repository: [xmake-repo](https://github.com/xmake-io/xmake-repo)

## Build project

```bash
$ xmake
```

## Run target

```bash
$ xmake run console
```

## Debug target

```bash
$ xmake run -d console
```

## Configure platform

```bash
$ xmake f -p [windows|linux|macosx|android|iphoneos ..] -a [x86|arm64 ..] -m [debug|release]
$ xmake
```

## Menu configuration

```bash
$ xmake f --menu
```

<img src="https://xmake.io/assets/img/index/menuconf.png" width="650px" />

## Build as fast as ninja

The test project: [xmake-core](https://github.com/xmake-io/xmake/tree/master/core)

### Multi-task parallel compilation

| buildsystem     | Termux (8core/-j12) | buildsystem      | MacOS (8core/-j12) |
|-----            | ----                | ---              | ---                |
|xmake            | 24.890s             | xmake            | 12.264s            |
|ninja            | 25.682s             | ninja            | 11.327s            |
|cmake(gen+make)  | 5.416s+28.473s      | cmake(gen+make)  | 1.203s+14.030s     |
|cmake(gen+ninja) | 4.458s+24.842s      | cmake(gen+ninja) | 0.988s+11.644s     |

### Single task compilation

| buildsystem     | Termux (-j1)     | buildsystem      | MacOS (-j1)    |
|-----            | ----             | ---              | ---            |
|xmake            | 1m57.707s        | xmake            | 39.937s        |
|ninja            | 1m52.845s        | ninja            | 38.995s        |
|cmake(gen+make)  | 5.416s+2m10.539s | cmake(gen+make)  | 1.203s+41.737s |
|cmake(gen+ninja) | 4.458s+1m54.868s | cmake(gen+ninja) | 0.988s+38.022s |

## Package management

### Download and build

<img src="https://xmake.io/assets/img/index/package_manage.png" width="650px" />

### Processing architecture

<img src="https://xmake.io/assets/img/index/package_arch.png" width="650px" />

## Supported platforms

* Windows (x86, x64)
* macOS (i386, x86_64)
* Linux (i386, x86_64, cross-toolchains ..)
* Android (i386, x86_64, armv5te, armv7-a, armv8-a, arm64-v8a, mips)
* iOS (armv7, armv7s, arm64, i386, x86_64)
* WatchOS (armv7k, i386)
* MSYS (i386, x86_64)
* MinGW (i386, x86_64)
* Cygwin (i386, x86_64)
* SDCC (stm8, mcs51, ..)
* Cross (cross-toolchains ..)

## Supported Languages

* C
* C++
* Objective-C and Objective-C++
* Swift
* Assembly
* Golang
* Rust
* Dlang
* Cuda

## Supported Projects

* Static Library
* Shared Library
* Console
* Cuda Program
* Qt Application
* WDK Driver (umdf/kmdf/wdm)
* WinSDK Application
* MFC Application

## More Examples

Debug and release modes:

```lua
add_rules("mode.debug", "mode.release")

target("console")
    set_kind("binary")
    add_files("src/*.c")
    if is_mode("debug") then
        add_defines("DEBUG")
    end
```

Custom scripts:

```lua
target("test")
    set_kind("binary")
    add_files("src/*.c")
    after_build(function (target)
        print("hello: %s", target:name())
        os.exec("echo %s", target:targetfile())
    end)
```

Download and use packages in [xmake-repo](https://github.com/xmake-io/xmake-repo) or third-party repositories:

```lua
add_requires("tbox >1.6.1", "libuv master", "vcpkg::ffmpeg", "brew::pcre2/libpcre2-8")
add_requires("conan::OpenSSL/1.0.2n@conan/stable", {alias = "openssl", optional = true, debug = true}) 
target("test")
    set_kind("binary")
    add_files("src/*.c")
    add_packages("tbox", "libuv", "vcpkg::ffmpeg", "brew::pcre2/libpcre2-8", "openssl")
```

Qt QuickApp Program:

```lua
target("test")
    add_rules("qt.quickapp")
    add_files("src/*.cpp")
    add_files("src/qml.qrc")
```

Cuda Program:

```lua
target("test")
    set_kind("binary")
    add_files("src/*.cu")
    add_cugencodes("native")
    add_cugencodes("compute_30")
```

WDK/UMDF Driver Program:

```lua
target("echo")
    add_rules("wdk.driver", "wdk.env.umdf")
    add_files("driver/*.c") 
    add_files("driver/*.inx")
    add_includedirs("exe")

target("app")
    add_rules("wdk.binary", "wdk.env.umdf")
    add_files("exe/*.cpp")
```

More wdk driver program examples (umdf/kmdf/wdm), please see [WDK Program Examples](https://xmake.io/#/guide/project_examples?id=wdk-driver-program)

## Plugins

#### Generate IDE project file plugin（makefile, vs2002 - vs2019 .. ）

```bash
$ xmake project -k vsxmake -m "debug;release" # New vsproj generator (Recommended)
$ xmake project -k vs -m "debug;release"
$ xmake project -k cmake
$ xmake project -k ninja
$ xmake project -k compile_commands
```

#### Run the custom lua script plugin

```bash
$ xmake l ./test.lua
$ xmake l -c "print('hello xmake!')"
$ xmake l lib.detect.find_tool gcc
$ xmake l
> print("hello xmake!")
> {1, 2, 3}
< { 
    1,
    2,
    3 
  }
```

More builtin plugins, please see: [Builtin plugins](https://xmake.io/#/plugin/builtin_plugins)

Please download and install more other plugins from the plugins repository [xmake-plugins](https://github.com/xmake-io/xmake-plugins).

## IDE/Editor Integration

* [xmake-vscode](https://github.com/xmake-io/xmake-vscode)

<img src="https://raw.githubusercontent.com/xmake-io/xmake-vscode/master/res/problem.gif" width="650px" />

* [xmake-sublime](https://github.com/xmake-io/xmake-sublime)

<img src="https://raw.githubusercontent.com/xmake-io/xmake-sublime/master/res/problem.gif" width="650px" />

* [xmake-idea](https://github.com/xmake-io/xmake-idea)

<img src="https://raw.githubusercontent.com/xmake-io/xmake-idea/master/res/problem.gif" width="650px" />

* [xmake.vim](https://github.com/luzhlon/xmake.vim) (third-party, thanks [@luzhlon](https://github.com/luzhlon))

## Project Examples

Some projects using xmake:

* [tbox](https://github.com/tboox/tbox)
* [gbox](https://github.com/tboox/gbox)
* [vm86](https://github.com/tboox/vm86)
* [more](https://github.com/xmake-io/awesome-xmake)

## Example Video

<a href="https://asciinema.org/a/133693">
<img src="https://asciinema.org/a/133693.png" width="650px" />
</a>

## Contacts

* Email：[waruqi@gmail.com](mailto:waruqi@gmail.com)
* Homepage：[tboox.org](https://tboox.org)
* Community：[/r/tboox on reddit](https://www.reddit.com/r/tboox/)
* ChatRoom：[Char on telegram](https://t.me/tbooxorg), [Chat on gitter](https://gitter.im/tboox/tboox?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
* Source Code：[Github](https://github.com/xmake-io/xmake), [Gitee](https://gitee.com/tboox/xmake)
* QQ Group: 343118190(full), 662147501
* Wechat Public: tboox-os

## Thanks

This project exists thanks to all the people who have [contributed](CONTRIBUTING.md):
<a href="https://github.com/xmake-io/xmake/graphs/contributors"><img src="https://opencollective.com/xmake/contributors.svg?width=890&button=false" /></a>

* [TitanSnow](https://github.com/TitanSnow): provide the xmake [logo](https://github.com/TitanSnow/ts-xmake-logo) and install scripts
* [uael](https://github.com/uael): provide the semantic versioning library [sv](https://github.com/uael/sv)
* [OpportunityLiu](https://github.com/OpportunityLiu): improve cuda, tests and ci

## Backers

Thank you to all our backers! 🙏 [[Become a backer](https://opencollective.com/xmake#backer)]

<a href="https://opencollective.com/xmake#backers" target="_blank"><img src="https://opencollective.com/xmake/backers.svg?width=890"></a>

## Sponsors

Support this project by becoming a sponsor. Your logo will show up here with a link to your website. [[Become a sponsor](https://opencollective.com/xmake#sponsor)]

<a href="https://opencollective.com/xmake/sponsor/0/website" target="_blank"><img src="https://opencollective.com/xmake/sponsor/0/avatar.svg"></a>
<a href="https://opencollective.com/xmake/sponsor/1/website" target="_blank"><img src="https://opencollective.com/xmake/sponsor/1/avatar.svg"></a>
<a href="https://opencollective.com/xmake/sponsor/2/website" target="_blank"><img src="https://opencollective.com/xmake/sponsor/2/avatar.svg"></a>
<a href="https://opencollective.com/xmake/sponsor/3/website" target="_blank"><img src="https://opencollective.com/xmake/sponsor/3/avatar.svg"></a>
<a href="https://opencollective.com/xmake/sponsor/4/website" target="_blank"><img src="https://opencollective.com/xmake/sponsor/4/avatar.svg"></a>
<a href="https://opencollective.com/xmake/sponsor/5/website" target="_blank"><img src="https://opencollective.com/xmake/sponsor/5/avatar.svg"></a>
<a href="https://opencollective.com/xmake/sponsor/6/website" target="_blank"><img src="https://opencollective.com/xmake/sponsor/6/avatar.svg"></a>
<a href="https://opencollective.com/xmake/sponsor/7/website" target="_blank"><img src="https://opencollective.com/xmake/sponsor/7/avatar.svg"></a>
<a href="https://opencollective.com/xmake/sponsor/8/website" target="_blank"><img src="https://opencollective.com/xmake/sponsor/8/avatar.svg"></a>
<a href="https://opencollective.com/xmake/sponsor/9/website" target="_blank"><img src="https://opencollective.com/xmake/sponsor/9/avatar.svg"></a>



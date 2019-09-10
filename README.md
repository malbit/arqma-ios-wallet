# Installation

**Before building iOS Libraries, please have a look at the [Pre-requisites](#pre-requisites)**

1. Clone the repository.
```sh
git clone https://github.com/fotolockr/CakeWallet.git
```
2. Install [homebrew](https://brew.sh/)
3. Install arqma dependencies:
```sh
brew install pkgconfig
brew install cmake
brew install zeromq
```
4. Build the arqma iOS libraries.
```sh
./install.sh
./build.sh
```
5. Install dependencies from Pod.
```sh
pod install
```

### Shared library building problems

You may get an error such as:
```
ld: symbol(s) not found for architecture armv7
```

If you get this issue then make sure that boost has been correctly built. You can check this by seeing if it exists at `Libraries/boost/builds/libs`. If the folder doesn't exist then run `./scripts/install_boost.sh` in the root director and check to see if any errors occur there.

### XCode Building problems
If you're having problems building with Xcode 10 or above then change to the `Legacy Build System`

# TEMP FIX TO BUILD ARQMA SHARED LIBRARIES

Currently you will fail to build the arqma shared libraries because of some errors.
Here are fixes that you should apply:

src/crypto/cn_heavy_hash.hpp:60
```diff
- #if defined(__aarch64__)
+ #if(0)
#pragma GCC target ("+crypto")
#include <sys/auxv.h>
```

CMakeLists_IOS.txt:28
```diff
set (IOS True)
+ set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D IOS")
```

CMakeLists.txt:555
```diff
  elseif(IOS AND ARCH STREQUAL "arm64")
    message(STATUS "IOS: Changing arch from arm64 to armv8")
    set(ARCH_FLAG "-march=armv8")
+ elseif(IOS AND ARCH STREQUAL "x86_64")
+   message(STATUS "IOS: Changing arch from x86_64 to x86-64")
+   set(ARCH_FLAG "-march=x86-64")
  else()
    set(ARCH_FLAG "-march=${ARCH}")
    if(ARCH STREQUAL "native")

```

src/wallet/CMakeLists.txt:77
```diff
- if (NOT ARQMA_DAEMON_AND_WALLET_ONLY)
+ if (0)
```

cmake/CheckTrezor.cmake:32
```diff
# Use Trezor master switch
+ if (USE_DEVICE_TREZOR)
- if (0)
```

# Testnet

To build the application from testnet, you need to set the `TESTNET` pre-processor macro to true.
You can do this by doing the following:
1. Select your project (Make sure you are not selecting a target)
2. Go to Build Settings
3. Search "Preprocessor Macros"
4. Add `USE_TESTNET=1` to `DEBUG` or `RELEASE` depending on what your needs are.
5. Search "Swift Compiler - Custom Flags"
6. Add `USE_TESTNET` under `Active Compilation Conditions`

> We use forked the repo of [ofxiOSBoost](https://github.com/Mikunj/ofxiOSBoost/). We do this ONLY for more convenient installation process.

# How to compile Scribus-1.7.x on macOS
How to compile Scribus 1.7.x on macOS

After these instrucations, you will have an up and running development environment where you can change source code and recompile it, and start your every new Scribus and enjoy your changes you made a minute ago.


## Document History

### 2025-06jun-07 (DRAFT)

- Limitaion. We had to left out (commented out on the cmake build system manually) podofo because Version 1.0.0 of podofo does not work right now out of the box with Scribus 1.7.1 source code.

- TODO: Solution? Install podofo lower version with Conan? https://conan.io/downloads
https://conan.io/center/recipes/podofo


### Limitations

This limitation is because Homebrew (brew) used here installed with:

```
% brew install podofo
```

- but this will install version 1.0.0 - but a lower version is known to work with Scribus.


- IMPORTANT: Homebrew (brew) and MacPorts should not be run and installed togehter. This will crash your OS installation.


#### TODO: Someone needs to port Scribus Code to be compatible with podofo 1.0.0

pdflib_core.cpp

```
#if (PODOFO_VERSION >= PODOFO_MAKE_VERSION(0, 10, 0))
```










## Overview

OS (macOS)
- OS (About This Mac): macOS Sequoia 15.5
- macOS SDK: % xcrun --sdk macosx --show-sdk-path /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX15.2.sdk
- Xcode: Xcode 16.2 (newer should also work, 16.3, 16.4)

User Interface (Qt)
- Qt6 version 6.8.3.

Additional software used
- brew (Homebrew) - to install additional software and libraries for macOS.

#### NOTES
- EDIT file.txt: This means that you need to edit the file named file.txt with a plain text editor like vi, pico, nano, Visual Studio Code, or whatever plain text editor you use.

#### NOTES about the Source Code style
- The source code uses an old unsafe style. If statments without enclosing brackets {}, etc.
- Indentation. It uses TAB as indent. Better whould be two spaces. All big tech companies does this for a reason. So use 2 spaces it. But here use TAB and setup your editor to display TAB as 2 spaces and preserve TABs.







## Step 1: Install Qt6

- We downloaded Qt 6.9.1 here but used Qt 6.8.3 libs only with Qt 5 Compatibility module.
- This setup uses about 7 GB of disk space. (If you add iOS support, Qt will take about 15 GB of disk space, iOS support is not reuqired here. If you do not use iOS and need to safe disk space, remove iOS support)
- Make sure you download the right version of Qt, the open source free version, NOT the free to try commercial version.


### Download Qt open source version

- https://www.qt.io/download-open-source
- Go to https://www.qt.io/download-open-source
- Hit "Download Qt Online Installer" button, it will take just to this Url:

https://www.qt.io/download-qt-installer-oss

- "Download Qt for open source use": Choose your OS: macOS. (macOS, Windows x64, Linux x64, Linux ARM64, Windows ARM64)
- Hit button "Qt Online Installer for macOS". This will point to the DMG disk image the macOS installer file, for example "qt-online-installer-macOS-x64-4.9.0.dmg".
- Download this, install the DMG.



### Run the Qt online installer app.

- Login with qt account.
- IF you do not have an Qt Account, Create new Qt account, email required.
- IF you have already installed Qt, Start Qt MaintenanceTool.app.

- Login, choose Qt then choose Qt 6.8.3.
- Select [x] Desktop 6.8.3 and [x] Qt 5 Compatibility module. (Also Qt Shader Tools was selected, but unsure if needed)
- Select [x] Build Tools, [x] CMake 3.30.5. (Nija 1.12.1 was also selected, but unsure if needed)
- Select [x] Qt Creator 16.0.2 (was pre selected)

Hit next and let the tool download and install what you have requested.

You can change these settings at anytime afterwards with the Qt MaintenanceTool.app tool.

Now you should have an up and running Qt installation.


NOTES
 - https://www.qt.io/ - Qt multiplatform developer tools and libs with a commmercial and a free open source license. All the tools you need for creating software applications or embedded devices, from planning and design to development, testing, and future-proofing your products.
 - cmake - https://cmake.org/ - CMake: A Powerful Software Build System. CMake is the de-facto standard for building C++ code, with over 2 million downloads a month. It’s a powerful, comprehensive solution for managing the software build process. Get everything you need to successfully leverage CMake by visiting our resources section.








## Step 2: Install Homebrew (brew) utils

Homebrew (brew) - The Missing Package Manager for macOS (or Linux)

If you not have brew installed, install it. See https://brew.sh/

```
% brew install svn
% brew install boost
% brew install podofo
% brew install hunspell
```

Footnotes

- Homebrew (brew) - https://brew.sh/ - The Missing Package Manager for macOS (or Linux)
- SVN, Apache Subversion -  https://subversion.apache.org/ - Subversion source code control
- Boost - C++ Libs
- podofo - PoDoFo is a free portable C++ library to work with the PDF file format. https://github.com/podofo/podofo/tree/master
- hunspell - Spellchecker












## Step 3: Get the Source Code

The latest source code is stored and hosted on a SVN server.

Change to folder you want to have your source code.

```
% mkdir src
% cd src
% svn co svn://scribus.net/trunk/Scribus scribustrunk
```





## Step 4: Compile


```
% cd scribustrunk

% export LDFLAGS="-L/usr/local/opt/icu4c@77/lib"
% export CPPFLAGS="-I/usr/local/opt/icu4c@77/include"
% export PKG_CONFIG_PATH="/usr/local/opt/icu4c@77/lib/pkgconfig"

% export Qt6_DIR="/Volumes/DATA-2022/Apps/qt.io/2025-06jun-06--6.9.1/6.8.3/macos/lib/cmake/Qt6"
% export QT_DIR="/Volumes/DATA-2022/Apps/qt.io/2025-06jun-06--6.9.1/6.8.3/macos/lib/cmake/Qt6/"
```


### IF you need to compile WITHOUT PoDoFo (-DWITH_PODOFO=OFF)

Footnote
- podofo - PoDoFo is a free portable C++ library to work with the PDF file format. https://github.com/podofo/podofo/tree/master


```
cmake -DWITH_PODOFO=OFF  -DCMAKE_OSX_SYSROOT=macosx -DCMAKE_OSX_DEPLOYMENT_TARGET=15.0  -DBUILD_OSX_BUNDLE=1 -DWANT_CAIRO=1 -DCMAKE_INSTALL_PREFIX:PATH=/Volumes/Apps/Office/scribus.net/src-app/Scribus.App/Contents/ ../scribustrunk/
```



### If you need to compile WITH PdDoFo

Footnote
- podofo - PoDoFo is a free portable C++ library to work with the PDF file format. https://github.com/podofo/podofo/tree/master

This did not work on my system because brew profided only version 1.0.0 of PdDoFo which breaks the scribus code.

```
% cmake -DCMAKE_OSX_SYSROOT=macosx -DCMAKE_OSX_DEPLOYMENT_TARGET=15.0  -DBUILD_OSX_BUNDLE=1 -DWANT_CAIRO=1 -DCMAKE_INSTALL_PREFIX:PATH=/Volumes/Apps/Office/scribus.net/src-app/Scribus.App/Contents/ ../scribustrunk/
```


```
% make
```

Note: It will take about 15 minutes to compile all source code for the first time.


```
% make install
```



## Step 5: Run

- The App is placed in ./src-app/Scribus.App.app

- Finder, right click, show package contents. Navigate to ./Contents/ and copy Scribus1.7.1.svn.app (cmd+c)

- Finder, back, back.

- Hit cmd-v to place the Scribus1.7.1.svn.app here.

- This is the final app you can start with double click from within the Finder.





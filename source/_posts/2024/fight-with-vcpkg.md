 CMake Error at cmake_install.cmake:88 (if):
           if given arguments:
         
             "EXISTS" "C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/pkgs/sdl2_x64-windows/debug/" "cmake\"/SDL2Targets.cmake\""
         
           Unknown arguments specified


C:\Users\Weslie\AppData\Local\vcpkg\registries\git-trees\4ab64007873e4e383360819f5227ad0747c649d1\portfile.cmake



vcpkg_cmake_configure(
    SOURCE_PATH "${SOURCE_PATH}"
    ${configure_opts}
    OPTIONS ${FEATURE_OPTIONS}
        -DSDL_STATIC=${SDL_STATIC}
        -DSDL_SHARED=${SDL_SHARED}
        -DSDL_FORCE_STATIC_VCRT=${FORCE_STATIC_VCRT}
        -DSDL_LIBC=ON
        -DSDL_TEST=OFF
        -DSDL_INSTALL_CMAKEDIR="cmake"
        -DCMAKE_DISABLE_FIND_PACKAGE_Git=ON
        -DPKG_CONFIG_USE_CMAKE_PREFIX_PATH=ON
        -DSDL_LIBSAMPLERATE_SHARED=OFF
    MAYBE_UNUSED_VARIABLES
        SDL_FORCE_STATIC_VCRT
        PKG_CONFIG_USE_CMAKE_PREFIX_PATH
)


if(EXISTS "$ENV{DESTDIR}${CMAKE_INSTALL_PREFIX}/"cmake"/SDL2Targets.cmake")


CMake Error at cmake_install.cmake:104 (file):
           file called with unknown argument "cmake""".


file(INSTALL DESTINATION "${CMAKE_INSTALL_PREFIX}/"cmake"" TYPE FILE FILES "C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/blds/sdl2/x64-windows-dbg/CMakeFiles/Export/9778476dad71dd9ea5b94a8249d49cca/SDL2Targets.cmake")




error: https://gitlab.xiph.org/xiph/ogg/-/archive/v1.3.5/ogg-v1.3.5.tar.gz: WinHttpSendRequest failed with exit code 121
75. 发生了安全错误
note: If you are using a proxy, please ensure your proxy settings are correct.
Possible causes are:
1. You are actually using an HTTP proxy, but setting HTTPS_PROXY variable to https//address:port.
This is not correct, because https:// prefix claims the proxy is an HTTPS proxy, while your proxy (v2ray, shadowsocksr
, etc...) is an HTTP proxy.
Try setting http://address:port to both HTTP_PROXY and HTTPS_PROXY instead.
2. If you are using Windows, vcpkg will automatically use your Windows IE Proxy Settings set by your proxy software. See
: https://github.com/microsoft/vcpkg-tool/pull/77
The value set by your proxy might be wrong, or have same https:// prefix issue.
3. Your proxy's remote server is our of service.
If you believe this is not a temporary download server failure and vcpkg needs to be changed to download this file from
a different location, please submit an issue to https://github.com/Microsoft/vcpkg/issues
CMake Error at scripts/cmake/vcpkg_download_distfile.cmake:136 (message):
  Download failed, halting portfile.


https://gitlab.xiph.org/xiph/ogg/-/archive/v1.3.5/ogg-v1.3.5.tar.gz


C:\Users\Weslie\AppData\Local\vcpkg\downloads\xiph-ogg-v1.3.5.tar.gz





CMake Error at scripts/cmake/vcpkg_download_distfile.cmake:114 (message):
  Downloads are disabled, but
  'C:/Users/Weslie/AppData/Local/vcpkg/downloads/xiph-ogg-v1.3.5.tar.gz' does
  not exist.






CMake Error at CMakeLists.txt:163 (message):
  SDL2MIXER_VORBIS contains an invalid value (="VORBISFILE").  It must be one
  of STB;TREMOR;VORBISFILE.


C:/cmake-3.24.4-windows-x86_64/bin/cmake.exe C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/blds/sdl2-mixer/src/ease-2.8.0-5d1a2b7a04.clean -G "Visual Studio 17 2022" -Ax64 -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/pkgs/sdl2-mixer_x64-windows/debug -DFETCHCONTENT_FULLY_DISCONNECTED=ON -DSDL2MIXER_MIDI=OFF -DSDL2MIXER_MIDI_FLUIDSYNTH=OFF -DSDL2MIXER_FLAC=OFF -DSDL2MIXER_FLAC_LIBFLAC=OFF -DSDL2MIXER_MOD=OFF -DSDL2MIXER_MOD_MODPLUG=OFF -DSDL2MIXER_MP3=OFF -DSDL2MIXER_MP3_MPG123=OFF -DSDL2MIXER_OPUS=OFF -DSDL2MIXER_MP3_DRMP3=OFF -DSDL2MIXER_VENDORED=OFF -DSDL2MIXER_SAMPLES=OFF -DSDL2MIXER_DEPS_SHARED=OFF -DSDL2MIXER_OPUS_SHARED=OFF -DSDL2MIXER_VORBIS_VORBISFILE_SHARED=OFF -DSDL2MIXER_VORBIS="VORBISFILE" -DSDL2MIXER_FLAC_DRFLAC=OFF -DSDL2MIXER_MIDI_NATIVE=OFF -DSDL2MIXER_MIDI_TIMIDITY=OFF -DSDL2MIXER_MP3_DRMP3=OFF -DSDL2MIXER_MOD_XMP_SHARED=1 -Tv143 -DBUILD_SHARED_LIBS=ON "-DVCPKG_CHAINLOAD_TOOLCHAIN_FILE=C:/Program Files (x86)/Microsoft Visual Studio/2022/BuildTools/VC/vcpkg/scripts/toolchains/windows.cmake" -DVCPKG_TARGET_TRIPLET=x64-windows -DVCPKG_SET_CHARSET_FLAG=ON -DVCPKG_PLATFORM_TOOLSET=v143 -DCMAKE_EXPORT_NO_PACKAGE_REGISTRY=ON -DCMAKE_FIND_PACKAGE_NO_PACKAGE_REGISTRY=ON -DCMAKE_FIND_PACKAGE_NO_SYSTEM_PACKAGE_REGISTRY=ON -DCMAKE_INSTALL_SYSTEM_RUNTIME_LIBS_SKIP=TRUE -DCMAKE_VERBOSE_MAKEFILE=ON -DVCPKG_APPLOCAL_DEPS=OFF "-DCMAKE_TOOLCHAIN_FILE=C:/Program Files (x86)/Microsoft Visual Studio/2022/BuildTools/VC/vcpkg/scripts/buildsystems/vcpkg.cmake" -DCMAKE_ERROR_ON_ABSOLUTE_INSTALL_DESTINATION=ON -DVCPKG_CXX_FLAGS= -DVCPKG_CXX_FLAGS_RELEASE= -DVCPKG_CXX_FLAGS_DEBUG= -DVCPKG_C_FLAGS= -DVCPKG_C_FLAGS_RELEASE= -DVCPKG_C_FLAGS_DEBUG= -DVCPKG_CRT_LINKAGE=dynamic -DVCPKG_LINKER_FLAGS= -DVCPKG_LINKER_FLAGS_RELEASE= -DVCPKG_LINKER_FLAGS_DEBUG= -DVCPKG_TARGET_ARCHITECTURE=x64 -DCMAKE_INSTALL_LIBDIR:STRING=lib -DCMAKE_INSTALL_BINDIR:STRING=bin "-D_VCPKG_ROOT_DIR=C:/Program Files (x86)/Microsoft Visual Studio/2022/BuildTools/VC/vcpkg" -D_VCPKG_INSTALLED_DIR=C:/GitHub/OnscripterYuriRT/vcpkg_installed -DVCPKG_MANIFEST_INSTALL=OFF


C:/cmake-3.24.4-windows-x86_64/bin/cmake.exe C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/blds/sdl2-mixer/src/ease-2.8.0-5d1a2b7a04.clean -G "Visual Studio 17 2022" -Ax64 -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/pkgs/sdl2-mixer_x64-windows/debug -DFETCHCONTENT_FULLY_DISCONNECTED=ON -DSDL2MIXER_MIDI=OFF -DSDL2MIXER_MIDI_FLUIDSYNTH=OFF -DSDL2MIXER_FLAC=OFF -DSDL2MIXER_FLAC_LIBFLAC=OFF -DSDL2MIXER_MOD=OFF -DSDL2MIXER_MOD_MODPLUG=OFF -DSDL2MIXER_MP3=OFF -DSDL2MIXER_MP3_MPG123=OFF -DSDL2MIXER_OPUS=OFF -DSDL2MIXER_MP3_DRMP3=OFF -DSDL2MIXER_VENDORED=OFF -DSDL2MIXER_SAMPLES=OFF -DSDL2MIXER_DEPS_SHARED=OFF -DSDL2MIXER_OPUS_SHARED=OFF -DSDL2MIXER_VORBIS_VORBISFILE_SHARED=OFF -DSDL2MIXER_VORBIS=VORBISFILE -DSDL2MIXER_FLAC_DRFLAC=OFF -DSDL2MIXER_MIDI_NATIVE=OFF -DSDL2MIXER_MIDI_TIMIDITY=OFF -DSDL2MIXER_MP3_DRMP3=OFF -DSDL2MIXER_MOD_XMP_SHARED=1 -Tv143 -DBUILD_SHARED_LIBS=ON "-DVCPKG_CHAINLOAD_TOOLCHAIN_FILE=C:/Program Files (x86)/Microsoft Visual Studio/2022/BuildTools/VC/vcpkg/scripts/toolchains/windows.cmake" -DVCPKG_TARGET_TRIPLET=x64-windows -DVCPKG_SET_CHARSET_FLAG=ON -DVCPKG_PLATFORM_TOOLSET=v143 -DCMAKE_EXPORT_NO_PACKAGE_REGISTRY=ON -DCMAKE_FIND_PACKAGE_NO_PACKAGE_REGISTRY=ON -DCMAKE_FIND_PACKAGE_NO_SYSTEM_PACKAGE_REGISTRY=ON -DCMAKE_INSTALL_SYSTEM_RUNTIME_LIBS_SKIP=TRUE -DCMAKE_VERBOSE_MAKEFILE=ON -DVCPKG_APPLOCAL_DEPS=OFF "-DCMAKE_TOOLCHAIN_FILE=C:/Program Files (x86)/Microsoft Visual Studio/2022/BuildTools/VC/vcpkg/scripts/buildsystems/vcpkg.cmake" -DCMAKE_ERROR_ON_ABSOLUTE_INSTALL_DESTINATION=ON -DVCPKG_CXX_FLAGS= -DVCPKG_CXX_FLAGS_RELEASE= -DVCPKG_CXX_FLAGS_DEBUG= -DVCPKG_C_FLAGS= -DVCPKG_C_FLAGS_RELEASE= -DVCPKG_C_FLAGS_DEBUG= -DVCPKG_CRT_LINKAGE=dynamic -DVCPKG_LINKER_FLAGS= -DVCPKG_LINKER_FLAGS_RELEASE= -DVCPKG_LINKER_FLAGS_DEBUG= -DVCPKG_TARGET_ARCHITECTURE=x64 -DCMAKE_INSTALL_LIBDIR:STRING=lib -DCMAKE_INSTALL_BINDIR:STRING=bin "-D_VCPKG_ROOT_DIR=C:/Program Files (x86)/Microsoft Visual Studio/2022/BuildTools/VC/vcpkg" -D_VCPKG_INSTALLED_DIR=C:/GitHub/OnscripterYuriRT/vcpkg_installed -DVCPKG_MANIFEST_INSTALL=OFF




C:\Users\Weslie\AppData\Local\vcpkg\registries\git-trees\b28cae64adf73bec946de9f037724763eb2ef1b2\portfile.cmake



vcpkg_cmake_configure(
    SOURCE_PATH "${SOURCE_PATH}"
    OPTIONS
        ${FEATURE_OPTIONS}
        ${EXTRA_OPTIONS}
        -DSDL2MIXER_VENDORED=OFF
        -DSDL2MIXER_SAMPLES=OFF
        -DSDL2MIXER_DEPS_SHARED=OFF
        -DSDL2MIXER_OPUS_SHARED=OFF
        -DSDL2MIXER_VORBIS_VORBISFILE_SHARED=OFF
        -DSDL2MIXER_VORBIS=VORBISFILE
        -DSDL2MIXER_FLAC_DRFLAC=OFF
        -DSDL2MIXER_MIDI_NATIVE=OFF
        -DSDL2MIXER_MIDI_TIMIDITY=OFF
        -DSDL2MIXER_MP3_DRMP3=OFF
        -DSDL2MIXER_MOD_XMP_SHARED=${BUILD_SHARED}
)





harfbuzz
CMake Error at scripts/cmake/vcpkg_execute_required_process.cmake:127 (message):
    Command failed: C:/Users/Weslie/AppData/Local/vcpkg/downloads/tools/ninja/1.12.1-windows/ninja.exe install -v
    Working Directory: C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/blds/harfbuzz/x64-windows-dbg
    Error code: 1
    See logs for more information:
      C:\GitHub\OnscripterYuriRT\vcpkg_installed\vcpkg\blds\harfbuzz\package-x64-windows-dbg-out.log
      
      
      
C:\Program Files (x86)\Windows Kits\8.1\include\um\combaseapi.h(229): error C2065: “IUnknown”: 未声明的标识符
C:\Program Files (x86)\Windows Kits\8.1\include\um\combaseapi.h(229): error C2187: 语法错误: 此处出现意外的“IUnknown”
C:\Program Files (x86)\Windows Kits\8.1\include\um\combaseapi.h(229): error C3878: 语法错误:“expression”后出现意外标记“*”
C:\Program Files (x86)\Windows Kits\8.1\include\um\combaseapi.h(229): error C2760: 语法错误: 此处出现意外的“)”；应为“;”
C:\Program Files (x86)\Windows Kits\8.1\include\um\combaseapi.h(229): error C3878: 语法错误:“expression-statement”后出现意外标记“)”
最后一行是ninja: build stopped: subcommand failed.



  C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\VC\Tools\MSVC\14.44.35207\bin\HostX64\arm\link.exe /ERRORREPORT:QUEUE /OUT:".\CompilerIdC.exe" /INCREMENTAL:NO /NOLOGO kernel32.lib user32.lib gdi32.lib winspool.lib comdlg32.lib advapi32.lib shell32.lib ole32.lib oleaut32.lib uuid.lib odbc32.lib odbccp32.lib /MANIFEST /MANIFESTUAC:"level='asInvoker' uiAccess='false'" /manifest:embed /PDB:".\CompilerIdC.pdb" /SUBSYSTEM:CONSOLE /TLBID:1 /DYNAMICBASE /NXCOMPAT /IMPLIB:".\CompilerIdC.lib" /MACHINE:ARM Debug\CMakeCCompilerId.obj


gdi32.lib   ComDlg32.lib  odbc32.lib  odbccp32.lib  shell32.lib   WinSpool.lib


  C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\VC\Tools\MSVC\14.44.35207\bin\HostX64\arm\link.exe /ERRORREPORT:QUEUE /OUT:"C:\GitHub\OnscripterYuriRT\vcpkg_installed\vcpkg\blds\sdl2\arm-windows-dbg\Debug\SDL2d.dll" /INCREMENTAL /ILK:"SDL2.dir\Debug\SDL2d.ilk" /NOLOGO kernel32.lib user32.lib gdi32.lib winmm.lib imm32.lib ole32.lib oleaut32.lib version.lib uuid.lib advapi32.lib setupapi.lib shell32.lib kernel32.lib user32.lib /MANIFEST /MANIFESTUAC:"level='asInvoker' uiAccess='false'" /manifest:embed /DEBUG /PDB:"C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/blds/sdl2/arm-windows-dbg/Debug/SDL2d.pdb" /SUBSYSTEM:CONSOLE /TLBID:1 /DYNAMICBASE /NXCOMPAT /IMPLIB:"C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/blds/sdl2/arm-windows-dbg/Debug/SDL2d.lib" /MACHINE:ARM  /machine:ARM /nologo /DLL SDL2.dir\Debug\version.res


winmm.lib imm32.lib version.lib setupapi.lib   kernel32.lib


  C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\VC\Tools\MSVC\14.44.35207\bin\HostX64\arm\link.exe /ERRORREPORT:QUEUE /OUT:"C:\GitHub\OnscripterYuriRT\vcpkg_installed\vcpkg\blds\libwebp\arm-windows-dbg\Debug\libwebp.dll" /INCREMENTAL /ILK:"webp.dir\Debug\libwebp.ilk" /NOLOGO Debug\libsharpyuv.lib shlwapi.lib ole32.lib windowscodecs.lib kernel32.lib user32.lib /MANIFEST /MANIFESTUAC:"level='asInvoker' uiAccess='false'" /manifest:embed /DEBUG /PDB:"C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/blds/libwebp/arm-windows-dbg/Debug/libwebp.pdb" /SUBSYSTEM:CONSOLE /TLBID:1 /DYNAMICBASE /NXCOMPAT /IMPLIB:"C:/GitHub/OnscripterYuriRT/vcpkg_installed/vcpkg/blds/libwebp/arm-windows-dbg/Debug/libwebp.lib" /MACHINE:ARM  /machine:ARM /nologo /DLL "C:\GitHub\OnscripterYuriRT\vcpkg_installed\vcpkg\blds\libwebp\arm-windows-dbg\webpdecode.dir\Debug\alpha_dec.obj"

shlwapi.lib


Downloading https://gitlab.freedesktop.org//freetype/freetype/-/archive/VER-2-13-3/freetype-VER-2-13-3.tar.gz -> freetype-freetype-VER-2-13-3.tar.gz
# smartmontools_osx
This is a temporary repository to provide official OSX package for the smartmontools
## How it works
Smartmontools package is cross-compiled in the Linux using [osxcross](https://github.com/tpoechtrager/osxcross) project. Resulted Mach-O universal binary is created for 2 architectures: i386 and x86_64, so it can be run on all 10.5+ intel based OSX systems. After compilation it is packed to the OSX pkg package and packed to the dmg image. 
## Implementation details
To create build environment i used Docker container. OSXCROSS project requires Apple SDK to work. SDK could be obtained from Apple Xcode. This is only non-free package in the build process. I am using Xcode 6.4 downloaded from the Apple developer website (apple id required). 
After osxcross we are building some other software required to build final package:
- libdmg - used to convert image from ISO to HFS+ format
- bomutils - tool to create osx "bill of materials", required to build pkg
- xar - archiever used to pack pkg

All this work is done in the [build_env Dockerfile](https://github.com/samm-git/smartmontools_osx/blob/master/build_env/Dockerfile). 
When build env is built we are cross-compiling smartmontools and creating pkg file, which is later packed into dmg disk image. This is done in the [smartmontools Docker env](https://github.com/samm-git/smartmontools_osx/blob/master/smartmontools/).
## To do
- provide launchctl script and instructions to run smartd as daemon

## Limitations
- Package can be installed only to the system root (/) partition. I disabled installation to the user volumes, because it is not clear what to do with configuration and uninstaller in this case. May be i will re-enable this later.
- PPC is not supported. I dont have such hardware to test and probably it is very rare now
- Smartmontools/Darwin itself is lack many features available on other platforms, because of Darwin limitations

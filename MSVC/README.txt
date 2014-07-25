---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

HOWTO

A.)
You should start by downloading these third-party dependencies (you should prefer .zip or .7z over .tar.gz and .bz archive types if possible -> see later *):
1:
BerkeleyDB - the project file included in the solution was made for and tested with DB 6.1.19NC
http://www.oracle.com/technetwork/database/database-technologies/berkeleydb/downloads/index.html
2:
Boost - you need 1.56.0-b1 or later (the wallet won't compile with <=1.55.0 unless you selectively apply the proposed patches to fix some known MSVC related Boost 1.55.0 bugs).
http://www.boost.org/users/download/
3:
libpng - our project file was made for / tested with 1.6.12
http://www.libpng.org/pub/png/libpng.html
4:
miniupnpc - our project file was made for / tested with 1.9.20140701
http://miniupnp.free.fr/files/
5:
openssl - our project file was made for / tested with 1.0.1.h
http://www.openssl.org/source/
6:
qrencode - you need the win32 fork (as of this date, the original code won't compile in MSVC) - our project file was made for / tested with Rev 681f2ea7a41f (Aug 30, 2013).
https://code.google.com/p/qrencode-win32/source/checkout
7:
Nokia Qt - you need 5.1.0 or later (they changed their default wchart setup) - our project file was made for and tested with 5.3.1.7
8:
zlib - our project file was made for and tested with 1.2.8
http://zlib.net/

B.)
Now you need to unpack everything into the .\GoldReserve\src directory. Here is safe way to achieve the expected results (some tarballs are tricky to unpack on Windows):
1:
- You should copy all the archive files listed above (8 in total) into the GoldReserve\src directory.
- You should launch your favorite archive manager (WinRAR, 7zip, etc.) with Administrator privileges (this is mandatory for the OpenSSL tarball).
- You should select the archive files (all the 8) and hit Extract (or Unpack, or similar...), possibly in silent mode (like the "without confirmation in WinRAR).
* Note that while some tarballs "only" require Administrator privileges, some of them won't properly unpack on Windows anyway (varies between archive managers).
This is why you are better off with any other archive types on Windows (.zip, .7z, .rar, etc). But as of this date, OpenSSL is tarball only, hence the Admin notes...
2:
You need to strip the version numbers form the name of the freshly unpack directories. They should read exactly as follows:
boost, db, libpng, miniupnpc, openssl, qrencode, qt-everywhere-opensource-src, zlib
-> Important note: you probably need to move the qrencode stuff by one or two directories up from the lower subdirectorie(s) (depending on how you obtained the files).
                   MSVC will look for files like: .\GoldReserve\src\qrencode\qrencode.c (NOT .\GoldReserve\src\qrencode\qrencode-win32\qrencode.c).
Also note that you already have a "qt" folder (the wallet GUI code), so do not strip too much from the qt-everywhere-opensource-src-x.x.x.x name, only the versioning.
This is important if you wish to use our project files without further manual configuration. MSVC will look for these folders (all of them, with this exact name).

C.)
You need to install CMake (I mean properly install it, system wide, with it's own installer, not just unpack it into the GoldReserve directory or somewhere else).
To be precise, CMake isn't mandatory. It's used by some project files as a custom pre-build step to carry out some pre-build configurations on third-party code.
If you are so inclined, you can disable these pre-build steps and do that very minimal configuration manually (it's often enough to strip the .in from some file names).
However, using CMake is the proper way to do this, so please install and use it... This is also necessary for the seamless "automatic" building of the MSVC solution.
http://www.cmake.org/cmake/resources/software.htm

D.)
You need to install Perl - we used ActivePerl 5.16.3, other Perl variants might and other (newer) ActivePerl versions should also work
http://www.activestate.com/activeperl/downloads

E.)
You can load the .sln file now. Hopefully it will load and every projects will compile without any kind of errors. :)
Oh, LOL, you know they won't. :P But they probably will after you read the notes again and now you decide to follow it step-by-step. :D

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

NOTES

These project files were made for and tested with MSVC 2013 (v12).
Older MSVC versions might or might not work (updated v10 and v11 versions will probably work but you need to manually adapt some version specific settings accordingly).

It is important to note that our Qt project has a custom build step which backs up and replaces the Qt build configuration file with our own uniquely modified version.
This was necessary to statically link the final exe file. Future versions of Qt might (or might not) offer an official config switch to achive this without manual editing
and it's also possible that this brute force editing will break the compilation of future Qt versions. Keep this in mind regarding any Qt related compilation errors...
This also means that you can't use your existing Qt build (unless you used a similar build configuration) and you can't use this unofficially built Qt in your other projects.
You can see what's overwritten by examining the MSVC\openssl\qmake.conf file (when I was there anyway, I made some small performance tweaks as well :). 

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

TODO

- Continuously maintain the project files as needed (make them compatible with the future changes in both the wallet's and/or the third-party dependency code changes)
- Add Debug and Win64 configurations (so 3 in total...)
- Refine the project files (this is the initial release, so I guess I left some redundant garbage in them)
- on demand / serve the requests from the community... -> preferably by merging the supplied patches :)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

THANKS / CREDITS

https://bitcoinqtmsvc2012.codeplex.com
I could reuse most of their code changes. :)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
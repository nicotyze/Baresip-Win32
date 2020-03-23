Environment
-----------
  - Tested with  
    - Windows 10
    - VirtualBox + Ubuntu 16.04.1 (prerequisites: git, svn, wget, "build-essential"...)


   - Useful links  

      https://github.com/alfredh/baresip-win32   
      https://github.com/alfredh/baresip


  - Starting point  

		git clone https://github.com/nicotyze/Baresip-Win32.git
		cd Baresip-Win32
		wget https://sourcesup.renater.fr/frs/download.php/file/6043/win_install.tar.gz
		tar xzf win_install.tar.gz

Build
----- 

 - Third Party Libraries  
  
	###### mingw32, wine ######
		sudo apt-get install mingw-w64-i686-dev wine
	###### OpenSSL ###### 
		wget https://www.openssl.org/source/openssl-1.1.0e.tar.gz
		tar xzf openssl-1.1.0e.tar.gz && mv openssl-1.1.0e openssl
		cd openssl
		./Configure mingw shared --cross-compile-prefix=i686-w64-mingw32- && make -j2
		export SSL_PATH=`pwd` && cd ..
	###### FFmpeg ######
		wget https://ffmpeg.zeranoe.com/builds/win32/dev/ffmpeg-3.4.1-win32-dev.zip
		unzip ffmpeg-3.4.1-win32-dev.zip
		cd ffmpeg-3.4.1-win32-dev
		export FFMPEG_PATH=`pwd` && cd ..
	###### SDL2 ######
		wget https://www.libsdl.org/release/SDL2-devel-2.0.7-mingw.tar.gz
		tar xzf SDL2-devel-2.0.7-mingw.tar.gz
		cd SDL2-2.0.7
		export SDL2_PATH=`pwd` && cd ..
	###### TIFF ######
		wget wget https://downloads.sourceforge.net/project/gnuwin32/tiff/3.8.2-1/tiff-3.8.2-1-lib.zip
		unzip tiff-3.8.2-1-lib.zip -d tiff-3.8.2-1-lib
		cd tiff-3.8.2-1-lib
		export TIFF_PATH=`pwd` && cd ..
	###### Spandsp ######
		wget https://sourcesup.renater.fr/frs/download.php/file/6040/spandsp-0.0.6_lib.tar.gz
		tar xzf spandsp-0.0.6_lib.tar.gz
		cd spandsp-0.0.6_lib
		export SPANDSP_PATH=`pwd` && cd ..
	###### Sndfile ######
		wget https://sourcesup.renater.fr/frs/download.php/file/6042/libsndfile.tar.gz
		tar xzf libsndfile.tar.gz
		cd libsndfile
		export SNDFILE_PATH=`pwd` && cd ..
	###### Baresip Win32 ######
		wget https://raw.githubusercontent.com/alfredh/baresip-win32/master/Makefile
		patch -p1 < Makefile.diff

 - Baresip source code
  
		git clone https://github.com/alfredh/baresip.git
		git clone https://github.com/creytiv/re.git
		git clone https://github.com/creytiv/rem.git
	


 - Make  
  
		export EXTRA_CFLAGS="-I$SSL_PATH/include -I$FFMPEG_PATH/include -I$SPANDSP_PATH/src -I$SDL2_PATH/i686-w64-mingw32/include -I/$TIFF_PATH/include -I/$SNDFILE_PATH/include"
		export EXTRA_LIBS="-lgdi32 -lcrypt32 -lstrmiids  -loleaut32 -lole32 -lstdc++ $SDL2_PATH/i686-w64-mingw32/lib/libSDL2.dll.a $SPANDSP_PATH/spandsp.lib $SNDFILE_PATH/lib/libsndfile-1.lib -lpthread -lavcodec.dll -lavutil.dll -lavformat.dll -lavdevice.dll"
		export EXTRA_LFLAGS="-L$FFMPEG_PATH/lib -L$SPANDSP_PATH -L/usr/i686-w64-mingw32/lib"
		LIBS=$EXTRA_LIBS  make TUPLE=i686-w64-mingw32 baresip PREFIX=`pwd`/win_install  baresip USE_AVCODEC=1 USE_SDL=1 USE_AVFORMAT=1 USE_DSHOW=1 USE_G722=1 USE_SNDFILE=1 HAVE_PTHREAD=1 install


Usage
----- 

    cd win_install
    ./web4baresip.bat





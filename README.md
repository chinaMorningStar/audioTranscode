# audioTranscode
java+ffmpeg+linux音频转码，aac转mp3，小程序跟app端发送语音识别成文字


准备工作：
lame--libmp3lame的安装包，支持MP3编码；yasm--NASM的重写，用于编译ffmpeg。

 

1.下载
ffmpeg下载链接：http://ffmpeg.org/download.html

yasm下载链接：http://www.linuxfromscratch.org/blfs/view/svn/general/yasm.html

lame下载接：https://sourceforge.net/projects/lame/files/lame/

 

2.编译安装
2.1安装lame
tar -zxf lame-3.100.tar.gz
cd lame-3.100
./configure
make
make install
2.2安装yasm
tar -zxf yasm-1.3.0.tar.gz
cd yasm-1.3.0
sed -i 's#) ytasm.*#)#' Makefile.in
./configure
make
make install
2.3安装ffmpeg
tar -jxf ffmpeg-3.4.tar.bz2
cd ffmpeg-3.4
./configure --enable-shared --enable-libmp3lame
make
make install
 

3.配置调整
执行：ffmpeg --version

报错：ffmpeg: error while loading shared libraries: libavdevice.so.57: cannot open shared object file: No such file or directory

解决：编缉/etc/ld.so.conf，新建一行追加：/usr/local/lib；保存后执行命令重加载配置文件：ldconfig






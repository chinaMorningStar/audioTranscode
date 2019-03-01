# audioTranscode
java+ffmpeg+linux音频转码，aac转mp3，小程序跟app端发送语音识别成文字


准备工作：

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

4.导入转码工具jar包，jar包下载地址:
  链接: https://pan.baidu.com/s/1Ayt_5UJbT_gVEKkD9RGlHQ 提取码: s823 
  
下载后在maven中导入jar包   jar包为  jave-1.0.2.jar
                maven本地导入方法为   cmd 命令
                mvn install:install-file 
                    -Dfile=E:\jave-1.0.2.jar    //包的输入路径 
                    -DgroupId=jave 
                    -DartifactId=jave 
                    -Dversion=1.0.2 
                    -Dpackaging=jar 
                //执行完成后 jar 会放入maven 仓库中  maven/repository/jave/jave/1.0.2/
                pom.xml文件配置为
                <dependency>
                    <groupId>jave</groupId>
                    <artifactId>jave</artifactId>
                    <version>1.0.2</version>
                 </dependency>
  5.编写转码工具类
  public static void MavToMp3(String sources, String desFileName) {
        List<String> commend = new ArrayList<String>();
        commend.add("/usr/local/bin/ffmpeg");
        commend.add("-i");
        commend.add(sources);
        commend.add("-f");
        commend.add("mp3");
        commend.add("-acodec");
        commend.add("libmp3lame");
        commend.add("-y");
        commend.add(desFileName);
        StringBuffer test = new StringBuffer();
        for (int i = 0; i < commend.size(); i++)
            test.append(commend.get(i) + " ");
        System.out.println(test);
        ProcessBuilder builder = new ProcessBuilder();
        builder.command(commend);
        try {
            builder.redirectErrorStream(true);
            Process process = builder.start();
            if (process.isAlive()) {
                log.info("转码进行中" + System.currentTimeMillis());
            }
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        log.info("音频转换成功");
    }
               



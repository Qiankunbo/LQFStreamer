# LQFStreamer
## 项目特点
- 基于C++11开发，避免使用裸指针
- 支持桌面录制，声卡和麦克风录制
- 支持RTMP推流
- 支持RSP推流（在开发中）
- 支持桌面录制保存到本地
- 目前只支持Windows平台，后续再扩展到Linux平台

## 编译平台
visual studio 2015 + win10 ，目前只编译了win32的lib或dll，所以请使用**x86 debug**模式进行编译。

## 集成的第三方库
- fdk-aac  AAC编码库
- x264      H264编码库
- FFmpeg 用来记录  、转换数字音频、视频，并能将其转化为流的开源计算机程序，它包括了目前领先的音/视频编码库libavcodec。
- librtmp 开源的RTMP库，支持推流和拉流
- openssl   SSL 密码库工具
- portaudio 跨平台的音频播放和录音库

## 功能清单
- 屏幕录制
- 声音录制
- RTMP推流
- MP4文件保存
- Streamer支持RTP推送视频
- MediaPlayer支持RTP接收视频并播放
- MediaPlayer支持RTP接收音频并播放

## 后续任务
- 完善RTMP推流的异常处理
- 集成支持RTSP推流
- 集成RTMP服务器，供局域网客户端之间访问。
- RTSP推流器实现
- RTSP播放器实现
- RTMP播放器实现

## 编译要求
- vs2015或以上

## 使用方法
0. dll目录说明
- 运行程序需要的dll文件，请拷贝到编译目录(Debug目录)
1. 推送rtmp
- 启动桌面和声卡的采集
- 设置RTMP推流链接
- 启动RTMP推流

2. 录制MP4
- 启动桌面和声卡的采集
- 启动MP4视频录制

3. 目前的应用已在main.cpp里

## 测试
### RTP接收视频码流
使用MediaPlayer接收码流，使用ffmpeg进行推送
1. 使用ffmpeg进行推送
> ffmpeg -re -i out.h264 -vcodec libx264   -f rtp rtp://192.168.100.67:9000>test.sdp. 
> 192.168.100.67为自己的ip地址

2. 使用ffplay确认推流没有问题
>ffplay -protocol_whitelist "file,http,https,rtp,udp,tcp,tls" test.sdp

3. 使用MediaPlayer进行拉流播放
>需要修改RTP_Player.cpp中的 RTP_PlayerTest函数的listen_port端口为9000
可以接收ffmpeg推送的码流。

4. 使用Stream去推送桌面捕获的码流，配置main.cpp中的main_rtp_send_video()的dest_ip设置IP地址，以及dest_port端口。
### RTP接收音频码流
1. MediaPlayer中RTP_AAC_Receiver.cpp的RTP_AAC_Receiver_Test函数，用来测试接收音频码流，目前只支持AAC码流，默认支持的是LC AAC，44.1KHz，2通道。
2. 使用ffmpeg进行推送码流，统一设置输出为44.1kHz，2通道
```
// 
// 推送AAC文件
ffmpeg -re -i out.aac -c:a  aac -flags +global_header -ar 44100 -ac 2  -f rtp rtp://192.168.100.61:9004>audio.sdp

// 将MP3转成AAC格式
ffmpeg -re -i buweishui.mp3 -c:a  aac -flags +global_header -ar 44100 -ac 2  -f rtp rtp://192.168.100.61:9004>audio.sdp

// 推送音视频文件中的audio
ffmpeg -re -i dp19.mp4 -vn -c:a  aac -flags +global_header -ar 44100 -ac 2  -f rtp rtp://192.168.100.61:9004>audio.sdp
```
3. 测试的时候可以先用ffplay进行播放测试
```
ffplay -protocol_whitelist "file,http,https,rtp,udp,tcp,tls" audio.sdp
```
## 版权问题

本项目自有代码可以自由应用于各自商用、非商业的项目。
但是本项目也使用了部分其他的开源代码，在商用的情况下请自行替代或剔除；
由于使用本项目而产生的商业纠纷或侵权行为一概与本项项目及开发者无关，请自行承担法律风险。

## 联系方式
 - 邮箱：<592407834@qq.com>
 - QQ群：782508536
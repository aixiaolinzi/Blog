# 标题

## 版本信息

北京中科创达软件股份有限公司
ThunderSoft Co., Ltd.

版本  | 0.1| 
----|------|
制定部门   |	智能视觉北京研发中心	|
制定日期   |	2020-08-30	|
密    级   |	绝密	|
文档状态   |	初稿	|
修订人     |    邓宝成  |
修订日期   |	2020-08-30


## 目录
[toc]
### 1前言

### 2MediaRecorder状态图

### 3MediaRecorder状态机讲解

### 4具体方法讲解

### 5其他方法如下


## 参考文章
1.  [官方文档](https://developer.android.com/reference/android/media/MediaRecorder)


##  1前言

随着音视频开发的广泛应用，需要我们对MediaRecorder录制音频和视频的这个类。有更深刻的理解。接下来我们将详细的讲解MediaRecorder类的方法的使用

##  2状态图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200902095415118.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3R1bm1lbmdzbWlsZQ==,size_16,color_FFFFFF,t_70#pic_center)



##  3MediaRecorder状态机讲解


**Initial：**

初始状态，当使用new()方法创建一个MediaRecorder对象或者调用了


**reset()方法时**

该MediaRecorder对象处于Initial状态。在设定视频源或者音频源之后将转换为Initialized状态。另

外，在除Released状态外的其它状态通过调用reset()方法都可以使MediaRecorder进入该状态。

**Initialized:**

已初始化状态，可以通过在Initial状态调用setAudioSource()或setVideoSource()方法进入该状态。在这个状态可以通过setOutputFormat()方法设置输出格式，此时MediaRecorder转换为DataSourceConfigured状态。另外，通过reset()方法进入Initial状态。

**DataSourceConfigured:**


数据源配置状态，这期间可以设定编码方式、输出文件、屏幕旋转、预览显示等等。可以在Initialized状态通过setOutputFormat()方法进入该状态。另外，可以通过reset()方法回到Initial状态，或者通过prepare()方法到达Prepared状态。


**Prepared：**

就绪状态，在DataSourceConfigured状态通过prepare()方法进入该状态。在这个状态可以通过start()进入录制状态。另外，可以通过reset()方法回到Initialized状态。

**Recording：**

录制状态，可以在Prepared状态通过调用start()方法进入该状态。另外，它可以通过stop()方法或reset()方法回到Initial状态。

**Released：**

释放状态（官方文档给出的词叫做Idle state 空闲状态），可以通过调用release()方法来进入这个状态，这时将会释放所有和MediaRecorder对象绑定的资源。


**Error：**

错误状态，当错误发生的时候进入这个状态，它可以通过reset()方法进入Initial状态。

**特别说明：**使用MediaRecorder录音录像时需要严格遵守状态图说明中的函数调用先后顺序，在不同的状态调用不同的函数，否则会出现异常。


根据自己的真实文档编写。


##  4具体方法讲解


##### (1)setInputSurface(Surface surface) 

MediaRecorder 设置surface流，这个Surface 必需使用MediaCodec进行创建，创建的时机是在MediaRecorder实例创建之后进行创建。最后在创建成功之后设置给MediaRecorder.


```		
		Surface mRecorderSurface；
		MediaRecorder mMediaRecorder；
		if (mMediaRecorder == null) {
			mMediaRecorder = new MediaRecorder();
           mRecorderSurface =MediaCodec.createPersistentInputSurface();
         } else {
            // 重置到初始状态
            mMediaRecorder.reset();
           }
		 mMediaRecorder.setInputSurface(mRecorderSurface);
```

重新设置MediaRecorder的surface 的目的是我们在相机创建session 时将这个surface设置给底层sensor，使其画面绘制到我们创建的surface上，这样我们在操作开始录制，和停止录制时，操作的画面是我们传入的surface， 避免了我们开始录制和停止录制出现预览卡顿现象， 其次录制的surface只创建一次节省资源。
```	
        try {
            List<Surface> outputs = setVideoOutputSize(mTextureView.getSurfaceTexture(), mRecorderSurface);
                mCameraDevice.createCaptureSession(outputs, sessionStateCb, mBackgroundHandler);
        } catch (CameraAccessException | IllegalStateException e) {
            e.printStackTrace();
        }
```	

##### (2)setAudioSource(int audioSource) 
设置音频源，其取值如下：
 
```
MediaRecorder.AudioSource.MIC //设定录音来源为麦克风 （较常用设置）
MediaRecorder.AudioSource.CAMCORDER //设定录音来源与同方向的相机麦克风相同，若相机无内置相机或无法设定时，则使用预设的麦克风
MediaRecorder.AudioSource.DEFAULT // 默认音频源
MediaRecorder.AudioSource.VOICE_CALL//设定录音来源为语音拨出的语音与对方说话的声音
MediaRecorder.AudioSource.VOICE_COMMUNICATION //摄像头旁边的麦克风

```
##### (3)  setVideoSource(int audioSource) 

设置视频源，其取值如下：


```
MediaRecorder.VideoSource.DEFAULT 默认状态
MediaRecorder.VideoSource.SURFACE ，意思是从Surface里面读取画面去录制。（较常用设置）
MediaRecorder.VideoSource.CAMERA ，视频源来自于摄像头，也就是我们不单独设置surface的情况下使用
```
##### (4) setOutputFormat(int output_format)
用于设置输出格式
MediaRecorder.OutputFormat类中定义了setoutputFormat()方法的形参需要传入的一些常量值，这些值代表含义如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824184041822.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3R1bm1lbmdzbWlsZQ==,size_16,color_FFFFFF,t_70#pic_center)


```
//MPEG_4 是最常用的，因为输出常用格式：mp4，m4a；
mMediaRecorder.setOutputFormat(MediaRecorder.OutputFormat.MPEG_4);
```
 


##### (5) setOutputFile(String path)

设置录制的音频文件的保存位置。




##### (6) setAudioEncoder(int audio_encoder)
设置所录制的声音的编码格式

在MediaRecorder.AudioEncoder类中定义了调用常量如下：


（1）default：默认值

（2）AAC：高级音频编码，苹果用的就是这种音频格式，简单说下优缺点：优点:相对于mp3，AAC格式的音质更佳，文件更小。不足:AAC属于有损压缩	的格式，与时下流行的APE、FLAC等无损格式相比音质存在”本质上”的差距。加之，传输速度更快的USB3.0和16G  以上大容量MP3正在加速普	及，也使	得AAC头上”小巧”的光环不复存在。
	支持的容器格式（几乎支持所有输出格式）3GPP ，3gp，MPEG-4，mp4，m4a，ts （not seekable，Android 3.0+），aac
（3）HE_AAC：HE-AAC混合了AAC与SBR技术

（4）AAC_ELD: 低延时的AAC音频编解码器

（5）AMR_NB:编码的是无视频纯声音3gp文件就是amr,他的文件比AAC的小，他的音乐效果没ACC的好
	支持的文件类型/容器格式 .3GPP.3gp

（6）AMR_WB:VMR-WB 是新型可变速率多模式宽带语音编解码器，专为无线 CDMA 2000标准而设计，目的在于在 50 至 7000 HZ 的频带上进行语音	编码，采样率为 16 KHZ。VMR-WB 基于 3GPP AMR-WB (G722.2) 编解码器，在每秒速率12.65 Kbit 上可实现互操作。
	支持的文件类型/容器格式 .3GPP.3gp

**总结**：开发首选AAC就行了，如果录音被抢占了释放掉或者选default就不会出现这种问题。


##### (7) setVideoEncoder(int audio_encoder)

设置视频编码格式


在MediaRecorder.VideoEncoder类中定义了调用常量如下：

（1） default：默认编码
（2） H263：H263 多用于视频传输，其优点是压缩后体积小，占用带宽少；
（3） MPEG_4_SP:码率低代表它无需高码率即可有很好的视频效果，H264就更好了
（4） H264，也是用于网络视频传输，优点也和H263差不多；再是H264会比前两者更优秀一点，不过一般用在标清或者高清压缩比较	多。
（5） VP8: 据说比H264优秀。
（6）HEVC：一种新的视频压缩标准。可以替代H.264/ AVC编码标准。它将在H.264标准2至4倍的复杂度基础上，将压缩效率提升一倍以上。

##### (8)setVideoFrameRate(int rate)


设置要捕获的视频的帧速率。每秒要捕捉的视频帧数.必须在setVideoSource()后调用。在setOutputFormat()之后，在prepare()之前调用它


帧率或者称FPS（Frames Per Second，帧/秒），是指每秒显示的图片数，或者GPU处理时每秒能够更新的次数。越高的帧速率可以得到更流畅逼真的画面。每秒钟帧数越多，所显示的动作就会越流畅。帧率也会影响体积，帧率越高，每秒钟显示的画面越多，体积就越大。由于人类眼睛的特殊生理结构，如果画面帧率高于16，就会认为是连贯的，此现象称之为视觉暂留。并且当帧速达到一定数值后，再增长的话，人眼能感知的流畅度的提升就比较有限了。一般来说30fps是可以接受的，将性能提升至60fps则可以明显提升交互感和逼真感，如果超过75fps一般就不容易察觉到有明显的流畅度提升了。如果帧率超过屏幕刷新率只会浪费图形处理的能力。


其次设置的帧率不是恒定不变的，其受到环境亮度变化有所改变

##### (9)setCaptureRate(int rate)

设置要捕获的视频的帧速率 ，在延时情况下使用。setVideoFrameRate在正常记录情况下使用

##### (10)setAudioSamplingRate（int samplingRate）
常用的采样率有：
8,000 Hz - 电话所用采样率，对于人的说话已经足够
11,025 Hz
22,050 Hz - 无线电广播所用采样率
32,000 Hz - miniDV数码视频camcorder、DAT（LP mode）所用采样率
44,100 Hz - 音频CD,也常用于MPEG-1音频（VCD, SVCD, MP3）所用采样率
47,250 Hz - Nippon Columbia（Denon）开发的世界上第一个商用PCM录音机所用采样率
48,000 Hz - miniDV、数字电视、DVD、DAT、电影和专业音频所用的数字声音所用采样率
50,000 Hz - 二十世纪七十年代后期出现的3M和Soundstream开发的第一款商用数字录音机所用采样率
50,400 Hz - 三菱X-80数字录音机所用所用采样率
96,000或者192,000 Hz - DVD-Audio、一些LPCM DVD音轨、Blu-ray Disc（藍光碟）音轨、和HD-DVD（高清晰度DVD）音轨所用所用采样率
2.8224 MHz - SACD、索尼和飞利浦联合开发的称为Direct Stream Digital的1位sigma-delta modulation过程所用采样率。

**常用**：
	AAC音频编码标准支持的采样率范围在8 ~ 96khz范围内，
	AMRNB支持的采样率为8kHz
 	AMRWB支持的速率为16kHz



##### (11) setVideoEncodingBitRate（int bitRate）

视频比特率:指每秒传输速率(比特数(bit)。单位为bps(Bit Per Second)，比特率越高，每秒传送数据就越多，画质就越清晰。

可以说使用MediaRecorder的setVideoEncodingBitrate方法设置视频编码比特率，并传入请求的比特率，以位/秒为单位。视频的低比特率设置在256000位/秒（256kbps）范围之内，而高比特率视频在3000000位/秒（3 m kbps）范围之内。


#####(12) setAudioEncodingBitRate（int bitRate）

音频比特率:指将模拟声音信号转换成数字声音信号后，单位时间内的二进制数据量，是间接衡量音频质量的一个指标

作为参考，每秒8,000位（8 kbps）的比特率非常低，适用于需要通过慢速网络实时传输的音频，而每秒196,000位（196 kbps）或更高的速率并不罕见。 MP3文件中的音乐。当前，大多数Android设备仅在频谱的低端支持比特率，如果您选择的比特率太高，MediaRecorder会自动在其可支持的范围内选择一种比特率。
records.setAudioEncodingBitRate（8000）;


##   5其他方法如下

```	
getAudioSourceMax()	获取音频信号源的最高值。
getMaxAmplitude()	最后调用这个方法采样的时候返回最大振幅的绝对值
getMetrics()	返回当前Mediacorder测量的数据
getSurface()	当使用Surface作为视频源的时候，返回Sufrace对象
pause()	暂停录制
prepare()	准备录制
resume()	恢复录制
release()	释放与此MediaRecorder对象关联的资源
reset()	重新启动mediarecorder到空闲状态
setAudioChannels(int numChannels)	设置录制的音频通道数
setAudioEncoder(int audio_encoder)	设置audio的编码格式
setAudioEncodingBitRate(int bitRate)	设置录制的音频编码比特率
setAudioSamplingRate(int samplingRate)	设置录制的音频采样率
setAudioSource(int audio_source)	设置用于录制的音源
setAuxiliaryOutputFile(String path)	辅助时间的推移视频文件的路径传递
setAuxiliaryOutputFile(FileDescriptor fd)	在文件描述符传递的辅助时间的推移视频
setCamera(Camera c)	设置一个recording的摄像头，此方法在API21被遗弃，被getSurface替代
setCaptureRate(double fps)	设置视频帧的捕获率
setInputSurface(Surface surface)	设置持续的视频数据来源
setMaxDuration(int max_duration_ms)	设置记录会话的最大持续时间（毫秒）
setMaxFileSize(long max_filesize_bytes)	设置记录会话的最大大小（以字节为单位）
setOutputFile(FileDescriptor fd)	传递要写入的文件的文件描述符
setOutputFile(String path)	设置输出文件的路径
setOutputFormat(int output_format)	设置在录制过程中产生的输出文件的格式
setPreviewDisplay(Surface sv)	表面设置显示记录媒体（视频）的预览
setVideoEncoder(int video_encoder)	设置视频编码器，用于录制
setVideoEncodingBitRate(int bitRate)	设置录制的视频编码比特率
setVideoFrameRate(int rate)	设置要捕获的视频帧速率
setVideoSize(int width, int height)	设置要捕获的视频的宽度和高度
setVideoSource(int video_source)	开始捕捉和编码数据到setOutputFile（指定的文件）
setLocation(float latitude, float longitude)	设置并存储在输出文件中的地理数据（经度和纬度）
setProfile(CamcorderProfile profile)	指定CamcorderProfile对象
setOrientationHint(int degrees)	设置输出的视频播放的方向提示
setOnErrorListener(MediaRecorder.OnErrorListener l)	注册一个用于记录录制时出现的错误的监听器
setOnInfoListener(MediaRecorder.OnInfoListener listener)	注册一个用于记录录制时出现的信息事件

```	
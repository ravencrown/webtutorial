---
title: Video标签
category: htmlapi
layout: page
date: 2016-06-26
modifiedOn: 2016-06-26
---

## video标签在不同平台上的事件表现差异分析

为了文章的完整性，首先还是列举一下video标签的属性：

- src ：视频的属性
- poster：视频封面，没有播放时显示的图片
- preload：预加载
- autoplay：自动播放
- loop：循环播放
- controls：浏览器自带的控制条
- width：视频宽度
- height：视频高度

video 对象属性：

- audioTracks： 返回表示可用音频轨道的 AudioTrackList 对象。
- autoplay： 设置或返回是否在就绪（加载完成）后随即播放视频。
- buffered： 返回表示视频已缓冲部分的 TimeRanges 对象。
- controller： 返回表示视频当前媒体控制器的 MediaController 对象。
- controls： 设置或返回视频是否应该显示控件（比如播放/暂停等）。
- crossOrigin： 设置或返回视频的 CORS 设置。
- currentSrc： 返回当前视频的 URL。
- currentTime： 设置或返回视频中的当前播放位置（以秒计）。
- defaultMuted： 设置或返回视频默认是否静音。
- defaultPlaybackRate： 设置或返回视频的默认播放速度。
- duration： 返回视频的长度（以秒计）。
- ended： 返回视频的播放是否已结束。
- error： 返回表示视频错误状态的 MediaError 对象。
- height： 设置或返回视频的 height 属性的值。
- loop：设置或返回视频是否应在结束时再次播放。
- mediaGroup： 设置或返回视频所属媒介组合的名称。
- muted： 设置或返回是否关闭声音。
- networkState： 返回视频的当前网络状态。
- paused： 设置或返回视频是否暂停。
- playbackRate： 设置或返回视频播放的速度。
- played： 返回表示视频已播放部分的 TimeRanges 对象。
- poster： 设置或返回视频的 poster 属性的值。
- preload： 设置或返回视频的 preload 属性的值。
- readyState： 返回视频当前的就绪状态。
- seekable： 返回表示视频可寻址部分的 TimeRanges 对象。
- seeking： 返回用户当前是否正在视频中进行查找。
- src： 设置或返回视频的 src 属性的值。
- startDate： 返回表示当前时间偏移的 Date 对象。
- textTracks： 返回表示可用文本轨道的 TextTrackList 对象。
- videoTracks： 返回表示可用视频轨道的 VideoTrackList 对象。
- volume： 设置或返回视频的音量。
- width ：设置或返回视频的 width 属性的值。

video 对象方法：

- addTextTrack()： 向视频添加新的文本轨道。
- canPlayType()： 检查浏览器是否能够播放指定的视频类型。
- load()： 重新加载视频元素。
- play()： 开始播放视频。
- pause()： 暂停当前播放的视频。

然后列出可以用于视频状态监控的Media 事件（由媒介（比如视频、图像和音频）触发的事件，适用于所有html元素，但常用于 audio、embed、img、object 以及 video中）：

属性|值|描述
-------|--------|------
onabort|script|在退出时运行的脚本
oncanplay|script|当文件就绪可以开始播放时运行的脚本（缓冲已足够开始时）
oncanplaythrough|script|当媒介能够无需因缓冲而停止即可播放至结尾时运行的脚本
ondurationchange|script|当媒介长度改变时运行的脚本
onemptied|script|当发生故障并且文件突然不可用时运行的脚本（比如连接意外断开时）
onended|script|当媒介已到达结尾时运行的脚本（可发送类似“感谢观看”之类的消息）
onerror|script|当在文件加载期间发生错误时运行的脚本
onloadeddata|script|当媒介数据已加载时运行的脚本
onloadedmetadata|script|当元数据（比如分辨率和时长）被加载时运行的脚本
onloadstart|script|在文件开始加载且未实际加载任何数据前运行的脚本
onpause|script|当媒介被用户或程序暂停时运行的脚本
onplay|script|当媒介已就绪可以开始播放时运行的脚本
onplaying|script|当媒介已开始播放时运行的脚本
onprogress|script|当浏览器正在获取媒介数据时运行的脚本
onratechange|script|每当回放速率改变时运行的脚本（比如当用户切换到慢动作或快进模式）
onreadystatechange|script|每当就绪状态改变时运行的脚本（就绪状态监测媒介数据的状态）
onseeked|script|当 seeking 属性设置为 false（指示定位已结束）时运行的脚本
onseeking|script|当 seeking 属性设置为 true（指示定位是活动的）时运行的脚本
onstalled|script|在浏览器不论何种原因未能取回媒介数据时运行的脚本
onsuspend|script|在媒介数据完全加载之前不论何种原因终止取回媒介数据时运行的脚本
ontimeupdate|script|当播放位置改变时（比如当用户快进到媒介中一个不同的位置时）运行的脚本
onvolumechange|script|每当音量改变时（包括将音量设置为静音）时运行的脚本
onwaiting|script|当媒介已停止播放但打算继续播放时（比如当媒介暂停已缓冲更多数据）运行脚本

这些Media 事件在不同平台下表现各异，事件触发的场景有差异，事件触发后Video对象属性的返回值也不尽相同，下面重点归纳其差异点，首先我们会给出结论，然后附上测试数据。 测试直接使用最简单的方式，在页面上添加video标签播放视频，视频设置循环播放属性loop。

## 时间差异分析

下面是播放一个短视频，在不同平台触发事件和获取属性的差异表现。

**PC**

\#|event|readyState|currentTime (s)|buffered (s)|duration (s)|视频状态
1|loadstart|NOTHING|0|-|-|-
2|suspend|NOTHING|0|-|-|-
3|play|NOTHING|0|-|-|-
4|waiting|NOTHING|0|-|-|-
5|durationchange|METADATA|0|5.35|7.91|获取到视频长度
6|loadedmetadata|METADATA|0|0.66|7.91|获取到元数据
7|loadeddata|ENOUGHDATA|0|0.66|7.91|-
8|canplay|ENOUGH_DATA|0|0.66|7.91|-
9|playing|ENOUGH_DATA|0|0.66|7.91|开始播放
10|canplaythrough|ENOUGH_DATA|0|0.66|7.91|可以流畅播放
11|progress|ENOUGH_DATA|0.11|3.68|7.91|持续下载
12|timeupdate|ENOUGH_DATA|0.14|4.44|7.91|播放进度变化
…|…|…|…|…|…|…
23|progress|ENOUGH_DATA|1.77|7.91|7.91|下载完毕
24|suspend|ENOUGH_DATA|1.77|7.91|7.91|-
25|timeupdate|ENOUGH_DATA|1.9|7.91|7.91|继续播放中
…|…|…|…|…|…|…
48|timeupdate|ENOUGH_DATA|7.7|7.91|7.91|-
49|timeupdate|ENOUGH_DATA|0|7.91|7.91|-
50|seeking|METADATA|0|7.91|7.91|-
51|waiting|METADATA|0|7.91|7.91|-
52|timeupdate|ENOUGH_DATA|0|7.91|7.91|-
53|seeked|ENOUGH_DATA|0|7.91|7.91|播放完毕进度回到起点
54|canplay|ENOUGH_DATA|0|7.91|7.91|-
55|playing|ENOUGH_DATA|0|7.91|7.91|循环播放
56|canplaythrough|ENOUGH_DATA|0|7.91|7.91|-
57|timeupdate|ENOUGH_DATA|0.19|7.91|7.91|-
…|…|…|…|…|…|…















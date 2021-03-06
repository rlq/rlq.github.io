---
layout: post
title: Perform Battery
category: Android
tags: [Android]
---


# Batter Historian
1. JVM记录系统的运行状态，将其存贮在堆转储Heap Dump文件中。
2. 可通过MAT分析OOM。
3. MAT统计 size:2.2MB Classes:3.3k Objects:50.1k ClassLoader:84 Unreachable Objects Histogram

### 1. 数据准备
1. adb kill-server,为了对电量做一个完整的记录。
2. adb devices连接手机

### 2. 数据收集
1. 记录唤醒锁的信息 
	* adb shell dumpsys batterystats --enable full-wake-history
	* 通过启用完全唤醒锁锁定报告，电池历史记录将在几小时内溢出，所以短时间3-4h测试。
2. adb shell dumpsys batterystats --reset
3. 以上操作相当于初始化操作，防止干扰。
4. 以下几个命令，将log信息导出：
	* adb bugreport > bugreport.txt
	* adb shell dumpsys batterystats > batterstats.txt
	* adb shell dumpsys batterystats > com.example.demo > batterystats.txt
5. 在Battery Historian V1上需要将以上log做转换
	* python historian.py -a bugreport.txt > battery.html
	* historian.py 在github上可下载使用
	* historian.py需与以上txt放在同一目录。
6. 在Battery Historian V2上直接将bugreport.txt上传到http://localhost:9999中生成文件报告,但需要docker搭建本地服务。
	
## 一 参数

### 1. 横坐标
1. 10，20 ... 代表秒，以1分钟为周期，到60秒变为0.
2. 横坐标是时间轴，统计数据是以重置为起点，获取bugreport内容的时间为终点。

### 2. 纵坐标
1. plugged 充电状态，是否进行了充电，以及充电的时间范围。
2. screen 屏幕是否点亮，睡眠状态和点亮状态下的电量使用信息。
3. top 那个app处于最上层，用来判断某个app对手机电量的影响。对比app的耗电量，
4. wake_loak 记录wake_lock模块的工作时间，是否有停止的时候。
5. running 界面的状态，判断是否处于idle的状态，用来判断无操作状态下的电量消耗。
6. wake_lock_in 哪个部件开始工作，以及工作的时间。
7. gps 是否开启。
8. phone_in_call 是否进行通话。
9. Sync 是否与后台同步，持续时间为多久。
10. Job 后台工作，比较Service的运行。
11. data_conn 数据连接方式的改变，edge说明采用GPRS的方式连接网络。可以看到使用的是2g, 3g, 4g还是wifi进行数据交互。不同连接对电量使用的影响。
12. status 电池状态信息，有充电，放电，未充电，已充满，未知等不同状态。
13. phone_single_stregth 手机信号状态的改变。记录手机信号的强弱变化，判断对电量的影响。
14. health 电池健康状态的信息，反应了这块电池的使用时间。
15. plug 充电方式，usb或者插座，以及显示连接的时间。不同充电方式对电量的影响。
16. historian 


### 3. 附图
时间线
![时间线](https://raw.githubusercontent.com/google/battery-historian/master/screenshots/timeline.png)

系统统计信息
![系统统计信息](https://raw.githubusercontent.com/google/battery-historian/master/screenshots/system.png)

应用统计信息
![应用统计信息](https://raw.githubusercontent.com/google/battery-historian/master/screenshots/app.png)


## 二 配置

### 1. docker
1. 下载 https://docs.docker.com/install/
2. 配置 docker run -p 9999:9999 gcr.io/android-battery-historian/stable:3.0 --port 9999
3. 默认访问 http://localhost:9999

#### 2. Go
1. 下载 http://golang.org/doc/install上
2. 配置

	```
	go get -u github.com/golang/protobuf/proto
	go get -u github.com/golang/protobuf/protoc-gen-go
	go get -u github.com/google/battery-histrizan
	cd $GOPATH/src/github.com/google/battery-historian/
	bash setup.sh
	go run cmd/battery-historian/battery-historian.go

	```
3. 默认访问 http://localhost:9999

#### 参考文章
1. [电量分析工具Battery Historian使用](https://blog.csdn.net/hpc19950723/article/details/54381246)
2. [Google Battery Historian](https://github.com/google/battery-historian)





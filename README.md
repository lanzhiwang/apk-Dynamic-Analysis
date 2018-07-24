# apk应用动态分析基本方法

## 准备工作
1. 工作目录中存在待运行的apk文件
2. 3a7f459a4f8d6716d68e5df63f611b8b.apk 是待运行的apk文件
3. vpn.apk 用于抓包
4. 所有的apk文件都要有执行权限

```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ ll
total 44788
drwxrwxr-x 2 lanzhiwang lanzhiwang     4096 7月  24 14:11 ./
drwxr-xr-x 7 lanzhiwang lanzhiwang     4096 7月  24 14:09 ../
-rw-rw-r-- 1 lanzhiwang lanzhiwang 42848622 7月  12 16:53 3a7f459a4f8d6716d68e5df63f611b8b.apk
-rwxrwxr-x 1 lanzhiwang lanzhiwang  2994825 7月  20 10:41 vpn.apk*
lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```

## 获取apk文件的包名和主应用名
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ aapt dump badging 3a7f459a4f8d6716d68e5df63f611b8b.apk
package: name='com.subject.ysh' versionCode='20170401' versionName='2.3' platformBuildVersionName='5.1.1-1819727'
sdkVersion:'11'
targetSdkVersion:'21'
...
launchable-activity: name='com.subject.ysh.activity.SplashActivity'  label='' icon=''
feature-group: label=''

lanzhiwang@lanzhiwang-dev:~/work/adb_work$
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ aapt dump badging vpn.apk
package: name='com.jude.interceptor' versionCode='1' versionName='1.0' platformBuildVersionName='6.0-2166767'
...
launchable-activity: name='com.jude.interceptor.ui.MainActivity'  label='影墙' icon=''

lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```

## 获取连接的手机设备或者模拟器
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb devices
List of devices attached
48db50a5b827    device

lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```

## 查看手机上已经安装的软件包
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb shell pm list packages -f | grep package:/data/
package:/data/dataapp/SohuNewsClient.apk=com.sohu.newsclient
package:/data/dataapp/UCBrowser.apk=com.UCMobile
package:/data/dataapp/CTAccount.apk=cn.com.chinatelecom.account
package:/data/dataapp/pre_enavi.apk=com.pdager
package:/data/dataapp/Qunar.apk=com.Qunar
package:/data/dataapp/ctclient.apk=com.ct.client
package:/data/dataapp/esurfing_WiFi.apk=com.akazam.android.wlandialer
package:/data/dataapp/SurfingClientKefu.apk=com.surfing.kefu
package:/data/dataapp/BestPay.apk=com.chinatelecom.bestpayclient
package:/data/dataapp/ReadingJoy.apk=com.iyd.reader.ReadingJoy
package:/data/dataapp/TYYD.apk=com.lectek.android.sfreader
package:/data/dataapp/icity.apk=cn.ffcs.wisdom.city
package:/data/dataapp/QYVideoClient.apk=com.qiyi.video
package:/data/dataapp/CTWallet.apk=chinatelecom.mwallet
package:/data/dataapp/cloud189.apk=com.cn21.ecloud
package:/data/dataapp/Huawei_eStore.apk=com.eshore.ezone
package:/data/dataapp/Huawei_Besttone.apk=com.besttone.hall
package:/data/dataapp/mail189.apk=com.corp21cn.mail189
package:/data/dataapp/pim.apk=com.chinatelecom.pim
package:/data/dataapp/libao.apk=com.corp21cn.flowpay
package:/data/dataapp/QQBrowser.apk=com.tencent.mtt
package:/data/dataapp/GaoDeMAP.apk=com.autonavi.minimap
package:/data/dataapp/iCartoon.apk=com.erdo.android.FJDXCartoon
package:/data/dataapp/egame.apk=cn.egame.terminal.client4g
package:/data/dataapp/TencentVideo.apk=com.tencent.qqlive
package:/data/dataapp/Com_TYSX_IKAN.apk=com.telecom.video.ikan4g
package:/data/dataapp/yixin.apk=im.yixin
package:/data/dataapp/yingyongbao.apk=com.tencent.android.qqplaza
package:/data/dataapp/PhotoShare.apk=com.huawei.gallery.photoshare
package:/data/dataapp/ITING.apk=com.imusic.iting
package:/data/dataapp/taobao.apk=com.taobao.taobao
lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```

## 安装apk文件
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 install ./vpn.apk
256 KB/s (2994825 bytes in 11.388s)
    pkg: /data/local/tmp/vpn.apk
Success
lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```

## 查看是否安装成功
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb shell pm list packages -f | grep com.jude.interceptor
package:/data/app/com.jude.interceptor-1.apk=com.jude.interceptor
lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```

```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 install ./3a7f459a4f8d6716d68e5df63f611b8b.apk
279 KB/s (42848622 bytes in 149.798s)
    pkg: /data/local/tmp/3a7f459a4f8d6716d68e5df63f611b8b.apk
Success
lanzhiwang@lanzhiwang-dev:~/work/adb_work$

lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb shell pm list packages -f | grep com.subject.ysh
package:/data/app/com.subject.ysh-1.apk=com.subject.ysh
lanzhiwang@lanzhiwang-dev:~/work/adb_work$

lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell pm list packages -3
package:com.subject.ysh
package:com.jude.interceptor
lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```

## 启动抓包服务
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell am startservice -n com.jude.interceptor/.ui.MyService --ei cmd 1
Starting service: Intent { cmp=com.jude.interceptor/.ui.MyService (has extras) }
```

## 停止抓包服务
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell am startservice -n com.jude.interceptor/.ui.MyService --ei cmd 2
Starting service: Intent { cmp=com.jude.interceptor/.ui.MyService (has extras) }
```

```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell am startservice -n com.jude.interceptor/.ui.MyService --ei cmd 1
Starting service: Intent { cmp=com.jude.interceptor/.ui.MyService (has extras) }
lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```


## 查看抓包服务是否运行成功
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell ps | grep "com.jude.interceptor"
u0_a128   10839 269   952880 56472 ffffffff 00000000 S com.jude.interceptor
lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```

## 启动待运行的apk应用
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell am start -n com.subject.ysh/com.subject.ysh.activity.SplashActivity
Starting: Intent { cmp=com.subject.ysh/.activity.SplashActivity }
lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```

## 查看应用运行是否成功
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell ps | grep "com.subject.ysh"
u0_a129   11172 269   980728 71152 ffffffff 00000000 S com.subject.ysh
u0_a129   11225 269   896892 39920 ffffffff 00000000 S com.subject.ysh:bdservice_v1
lanzhiwang@lanzhiwang-dev:~/work/adb_work$
```

## 模拟点击应用n次
```bash
lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell monkey -p com.jude.interceptor --pct-touch 100 100
Events injected: 100
## Network stats: elapsed time=350ms (0ms mobile, 0ms wifi, 350ms not connected)

lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell monkey -p com.jude.interceptor --pct-touch 100 100
Events injected: 100
## Network stats: elapsed time=192ms (0ms mobile, 0ms wifi, 192ms not connected)

lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell monkey -p com.jude.interceptor --pct-touch 100 100
Events injected: 100
## Network stats: elapsed time=435ms (0ms mobile, 0ms wifi, 435ms not connected)

lanzhiwang@lanzhiwang-dev:~/work/adb_work$ adb -s 48db50a5b827 shell monkey -p com.jude.interceptor --pct-touch 100 100
Events injected: 100
## Network stats: elapsed time=452ms (0ms mobile, 0ms wifi, 452ms not connected)
```

note:
模拟点击的另一种方法是：
```bash
adb -s 48db50a5b827 shell input tap 71, 115
# 71, 115是坐标位置
```




## 对应用进行截屏操作
```bash
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell screencap -p /sdcard/3a7f459a4f8d6716d68e5df63f611b8b.png
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell screencap -p /sdcard/3a7f459a4f8d6716d68e5df63f611b8b_01.png
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell screencap -p /sdcard/3a7f459a4f8d6716d68e5df63f611b8b_02.png
lanzhiwang@lanzhiwang-dev:~$
```

## 查看相关截屏后的文件
```bash
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell ls /sdcard/3a7f459a4f8d6716d68e5df63f611b8b.png
/sdcard/3a7f459a4f8d6716d68e5df63f611b8b.png
lanzhiwang@lanzhiwang-dev:~$
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell ls /sdcard/3a7f459a4f8d6716d68e5df63f611b8b_01.png
/sdcard/3a7f459a4f8d6716d68e5df63f611b8b_01.png
lanzhiwang@lanzhiwang-dev:~$
```

## 将截屏文件上传到本地
```bash
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 pull /sdcard/3a7f459a4f8d6716d68e5df63f611b8b.png ./3a7f459a4f8d6716d68e5df63f611b8b.png
309 KB/s (109604 bytes in 0.345s)
lanzhiwang@lanzhiwang-dev:~$
```

## 查看产生的pcap包
```bash
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell ls /sdcard/PacketCap
20180724_032636.pcap
# 将pcap包上传到本地
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 pull  /sdcard/PacketCap/20180724_032636.pcap ./20180724_032636.pcap
62 KB/s (6236 bytes in 0.097s)
lanzhiwang@lanzhiwang-dev:~$
```

## 删除产生的截屏文件和pcap文件
```bash
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell rm -f /sdcard/PacketCap/20180724_032636.pcap
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell rm -f /sdcard/3a7f459a4f8d6716d68e5df63f611b8b.png
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell rm -f /sdcard/3a7f459a4f8d6716d68e5df63f611b8b_01.png
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell rm -f /sdcard/3a7f459a4f8d6716d68e5df63f611b8b_02.png
lanzhiwang@lanzhiwang-dev:~$
```


## 查看网络请求的端口号
```bash
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell cat /proc/self/net/tcp
  sl  local_address rem_address   st tx_queue rx_queue tr tm->when retrnsmt   uid  timeout inode
lanzhiwang@lanzhiwang-dev:~$
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell cat /proc/self/net/tcp6
  sl  local_address                         remote_address                        st tx_queue rx_queue tr tm->when retrnsmt   uid  timeout inode
   0: 0000000000000000FFFF000003231BAC:9EB7 0000000000000000FFFF0000762B110E:0050 01 00000000:00000000 00:00000000 00000000  1000        0 82839 1 00000000 23 4 24 10 -1
   1: 0000000000000000FFFF000003231BAC:EBEF 0000000000000000FFFF00003D3E4E75:01BB 01 00000000:00000000 00:00000000 00000000  1000        0 94422 1 00000000 21 4 0 10 -1
   2: 0000000000000000FFFF000003231BAC:DE7E 0000000000000000FFFF000081450D1F:0050 01 00000000:00000000 00:00000000 00000000  1000        0 96658 1 00000000 21 0 0 10 -1
   3: 0000000000000000FFFF000003231BAC:9EB5 0000000000000000FFFF0000762B110E:0050 01 00000000:00000000 00:00000000 00000000  1000        0 91476 1 00000000 27 4 24 10 -1
   4: 0000000000000000FFFF000003231BAC:89B1 0000000000000000FFFF000030120431:01BB 01 00000000:00000000 00:00000000 00000000 10070        0 91275 1 00000000 21 4 0 10 -1
   5: 0000000000000000FFFF000003231BAC:9EB8 0000000000000000FFFF0000762B110E:0050 01 00000000:00000000 00:00000000 00000000  1000        0 89376 1 00000000 21 4 24 19 16
   6: 0000000000000000FFFF000003231BAC:8974 0000000000000000FFFF000030120431:01BB 01 00000000:00000000 00:00000000 00000000 10070        0 72260 1 00000000 24 4 0 10 -1
   7: 0000000000000000FFFF000003231BAC:BA41 0000000000000000FFFF00003D37C276:01BB 01 00000000:00000000 00:00000000 00000000  1000        0 86823 1 00000000 21 4 8 10 -1
   8: 0000000000000000FFFF000003231BAC:9EBB 0000000000000000FFFF0000762B110E:0050 01 00000000:00000000 00:00000000 00000000  1000        0 90404 1 00000000 21 4 24 19 16
   9: 0000000000000000FFFF000003231BAC:CDC0 0000000000000000FFFF00006839C276:1467 01 00000000:00000000 00:00000000 00000000  1000        0 102080 1 00000000 378 4 14 4 -1
lanzhiwang@lanzhiwang-dev:~$
```

## 停止apk应用
```bash
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell am force-stop com.subject.ysh
lanzhiwang@lanzhiwang-dev:~$
```

## 卸载apk应用
```bash
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 uninstall com.subject.ysh
Success
lanzhiwang@lanzhiwang-dev:~$
lanzhiwang@lanzhiwang-dev:~$ adb -s 48db50a5b827 shell pm list packages -f | grep com.subject.ysh
lanzhiwang@lanzhiwang-dev:~$
```
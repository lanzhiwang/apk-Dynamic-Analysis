## 静态检测方法汇总

### 广告检测
```bash
$ cp test.apk test.zip
$ unzip test.zip
$ ll
total 13048
drwxrwxr-x  5 lanzhiwang lanzhiwang    4096 8月  13 14:51 ./
drwxrwxr-x  3 lanzhiwang lanzhiwang    4096 8月  13 09:53 ../
-rw-rw-r--  1 lanzhiwang lanzhiwang    6800 5月  28  2015 AndroidManifest.xml
drwxrwxr-x  5 lanzhiwang lanzhiwang    4096 8月  13 14:51 assets/
-rw-rw-r--  1 lanzhiwang lanzhiwang 1588164 4月  30  2015 classes.dex
drwxrwxr-x  2 lanzhiwang lanzhiwang    4096 8月  13 14:51 META-INF/
drwxrwxr-x 12 lanzhiwang lanzhiwang    4096 8月  13 14:51 res/
-rw-rw-r--  1 lanzhiwang lanzhiwang   10992 5月  28  2015 resources.arsc
-rw-rw-r--  1 lanzhiwang lanzhiwang 5864265 8月  13 09:53 test.apk
-rw-rw-r--  1 lanzhiwang lanzhiwang 5864265 8月  13 14:51 test.zip
$ dexdump -f ./classes.dex
Processing './classes.dex'...
Opened './classes.dex', DEX version '035'
DEX file header:
magic               : 'dex\n035\0'
$ dexdump -f ./classes.dex | grep 'Class descriptor  :'
  Class descriptor  : 'Landroid/support/v4/accessibilityservice/AccessibilityServiceInfoCompat$AccessibilityServiceInfoVersionImpl;'
############## 广告预置信息
{
"Lcom/adchina/android/ads/": ["a.ad.adchina"],
"Lcom/energysource/szj/": ["a.ad.energysource"],
"Lcom/baidu/Ad": ["a.ad.baidu"],
"Lcom/kuguo/": ["a.ad.kuguo"],
"Lcom/adwo/adsdk/": ["a.ad.adwo"],
"Lcom/tencent/mobwin/": ["a.ad.tencentMobwin"]}
}
```

### apk基本信息
```
infos = {
'permission': [],
'package': '',
'version_code': '',
'version_name': '',
'name': '',
'file_size': '',
'md5': '',
'sdk_version': ''
}
```
```bash
$ aapt dump badging ./test.apk

```

### 行为检测
```bash
$ apktool d -f ./test.apk -o ./out_dir/ -p ./out_dir/
$
$ find ./out_dir/ -name *.smali
./out_dir/smali/org/anddev/andengine/opengl/buffer/BufferObject.smali
./out_dir/smali/org/anddev/andengine/opengl/buffer/BufferObjectManager.smali
./out_dir/smali/org/anddev/andengine/opengl/vertex/VertexBuffer.smali
./out_dir/smali/org/anddev/andengine/opengl/vertex/SpriteBatchVertexBuffer.smali
$
$ cat ./out_dir/smali/org/anddev/andengine/opengl/buffer/BufferObject.smali
$
$ cat ./out_dir/smali/org/anddev/andengine/opengl/buffer/BufferObject.smali | grep 'invoke'
$
$ cat ./out_dir/smali/org/anddev/andengine/opengl/buffer/BufferObject.smali | grep 'invoke' | grep 'init'
$ cat ./out_dir/smali/org/anddev/andengine/opengl/buffer/BufferObject.smali | grep 'invoke' | grep 'run'
$
```
### 检测apk是否加壳


### fsav

### senseword
note: 测试APK下载链接：http://api.np.mobilem.360.cn/redirect/down/?from=shengtian&appid=453800
```bash
$ ll ./ext/
total 324
drwxrwxr-x 2 lanzhiwang lanzhiwang   4096 8月  13 15:58 ./
drwxrwxr-x 7 lanzhiwang lanzhiwang   4096 8月  13 15:51 ../
-rw-rw-r-- 1 lanzhiwang lanzhiwang 303779 8月  13 15:51 rzx.check.sense.word.jar
-rw-rw-r-- 1 lanzhiwang lanzhiwang  12062 8月  13 15:51 sensewordlib
$
$ apktool d -f ./test.apk -o ./out_dir/ -p ./out_dir/
$ java -Xmx256m -Djava.ext.dirs=./ext/ -jar './ext/rzx.check.sense.word.jar' './ext/sensewordlib' './out_dir/res/'
[[sex]in[res/values/strings.xml]]
$
```




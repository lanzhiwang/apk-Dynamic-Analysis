# apk build

```
查看当前拥有的API的id号
root@1cdcb6828dd7:~# /usr/local/android-sdk/tools/android list target
Available Android targets:
----------
id: 1 or "android-19"
     Name: Android 4.4.2
     Type: Platform
     API level: 19
     Revision: 4
     Skins: WVGA800 (default), WXGA800-7in, WSVGA, WXGA800, WVGA854, WXGA720, WQVGA400, WQVGA432, HVGA, QVGA
 Tag/ABIs : default/armeabi-v7a, default/x86
----------
id: 2 or "android-21"
     Name: Android 5.0.1
     Type: Platform
     API level: 21
     Revision: 2
     Skins: WVGA800 (default), WXGA800-7in, WSVGA, WXGA800, WVGA854, WXGA720, WQVGA400, WQVGA432, HVGA, QVGA
 Tag/ABIs : default/armeabi-v7a, default/x86
----------
id: 3 or "android-22"
     Name: Android 5.1.1
     Type: Platform
     API level: 22
     Revision: 2
     Skins: WVGA800 (default), WXGA800-7in, WSVGA, WXGA800, WVGA854, WXGA720, WQVGA400, WQVGA432, HVGA, QVGA
 Tag/ABIs : default/armeabi-v7a, default/x86
root@1cdcb6828dd7:~#

创建简单的android项目
root@1cdcb6828dd7:~/AndroidStutio# /usr/local/android-sdk/tools/android create project --target android-22 --name demo --path ./demo --activity MainActivity --package com.demo.www
Created project directory: ./demo
Created directory /root/AndroidStutio/demo/src/com/demo/www
Added file ./demo/src/com/demo/www/MainActivity.java
Created directory /root/AndroidStutio/demo/res
Created directory /root/AndroidStutio/demo/bin
Created directory /root/AndroidStutio/demo/libs
Created directory /root/AndroidStutio/demo/res/values
Added file ./demo/res/values/strings.xml
Created directory /root/AndroidStutio/demo/res/layout
Added file ./demo/res/layout/main.xml
Created directory /root/AndroidStutio/demo/res/drawable-xhdpi
Created directory /root/AndroidStutio/demo/res/drawable-hdpi
Created directory /root/AndroidStutio/demo/res/drawable-mdpi
Created directory /root/AndroidStutio/demo/res/drawable-ldpi
Added file ./demo/AndroidManifest.xml
Added file ./demo/build.xml
Added file ./demo/proguard-project.txt
root@1cdcb6828dd7:~/AndroidStutio#
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio$ tree -a demo/
demo/
├── AndroidManifest.xml
├── ant.properties
├── bin
├── build.xml
├── libs
├── local.properties
├── proguard-project.txt
├── project.properties
├── res
│   ├── drawable-hdpi
│   │   └── ic_launcher.png
│   ├── drawable-ldpi
│   │   └── ic_launcher.png
│   ├── drawable-mdpi
│   │   └── ic_launcher.png
│   ├── drawable-xhdpi
│   │   └── ic_launcher.png
│   ├── layout
│   │   └── main.xml
│   └── values
│       └── strings.xml
└── src
    └── com
        └── demo
            └── www
                └── MainActivity.java

13 directories, 13 files
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio$
编译资源生成R.java文件
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ mkdir gen
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ aapt package -f -M ./AndroidManifest.xml -S ./res/ -J ./gen/ -m -I /usr/local/android-sdk/platforms/android-22/android.jar
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    gen/

nothing added to commit but untracked files present (use "git add" to track)
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ tree -a gen/
gen/
└── com
    └── demo
        └── www
            └── R.java

3 directories, 1 file
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
将 .java 文件编译成 .class 文件
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ javac -encoding UTF-8 -target 1.8 -bootclasspath /usr/local/android-sdk/platforms/android-22/android.jar -d bin ./src/com/demo/www/*.java ./gen/com/demo/www/R.java
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    bin/
    gen/

nothing added to commit but untracked files present (use "git add" to track)
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ tree -a gen/
gen/
└── com
    └── demo
        └── www
            └── R.java

3 directories, 1 file
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ tree -a bin/
bin/
└── com
    └── demo
        └── www
            ├── MainActivity.class
            ├── R$attr.class
            ├── R.class
            ├── R$drawable.class
            ├── R$layout.class
            └── R$string.class

3 directories, 6 files
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git add .
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git commit -m "aapt and javac"
[master 9850161] aapt and javac
 7 files changed, 22 insertions(+)
 create mode 100644 bin/com/demo/www/MainActivity.class
 create mode 100644 bin/com/demo/www/R$attr.class
 create mode 100644 bin/com/demo/www/R$drawable.class
 create mode 100644 bin/com/demo/www/R$layout.class
 create mode 100644 bin/com/demo/www/R$string.class
 create mode 100644 bin/com/demo/www/R.class
 create mode 100644 gen/com/demo/www/R.java
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
将 .class 文件编译成 .dex 文件
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ dx --dex --output=./bin/classes.dex ./bin/
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    bin/classes.dex

nothing added to commit but untracked files present (use "git add" to track)
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
将资源文件初始化
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ mkdir assets
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ aapt package -f -M ./AndroidManifest.xml -S ./res/ -A ./assets/ -I /usr/local/android-sdk/platforms/android-22/android.jar -F ./bin/resources
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    bin/classes.dex
    bin/resources

nothing added to commit but untracked files present (use "git add" to track)
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
打包生产APK包
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ java -cp /usr/local/android-sdk-linux/tools/lib/sdklib.jar com.android.sdklib.build.ApkBuilderMain ./Demo.apk -v -u -z ./bin/resources -f ./bin/classes.dex -rf src

THIS TOOL IS DEPRECATED. See --help for more information.

Packaging Demo.apk
./bin/resources:
=> AndroidManifest.xml
=> res/drawable-hdpi-v4/ic_launcher.png
=> res/drawable-ldpi-v4/ic_launcher.png
=> res/drawable-mdpi-v4/ic_launcher.png
=> res/drawable-xhdpi-v4/ic_launcher.png
=> res/layout/main.xml
=> resources.arsc
./bin/classes.dex => classes.dex
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    Demo.apk
    bin/classes.dex
    bin/resources

nothing added to commit but untracked files present (use "git add" to track)
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
生成私钥用于签名
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-alias
Enter keystore password: 
Re-enter new password:
What is your first and last name?
  [Unknown]:  lanzhiwang
What is the name of your organizational unit?
  [Unknown]:  Technical Support
What is the name of your organization?
  [Unknown]:  lanzhiwang
What is the name of your City or Locality?
  [Unknown]:  wuhan
What is the name of your State or Province?
  [Unknown]:  hubei
What is the two-letter country code for this unit?
  [Unknown]:  china
Is CN=lanzhiwang, OU=Technical Support, O=lanzhiwang, L=wuhan, ST=hubei, C=china correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA256withRSA) with a validity of 10,000 days
    for: CN=lanzhiwang, OU=Technical Support, O=lanzhiwang, L=wuhan, ST=hubei, C=china
Enter key password for <my-alias>
    (RETURN if same as keystore password): 
Re-enter new password:
[Storing my-release-key.jks]

Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore my-release-key.jks -destkeystore my-release-key.jks -deststoretype pkcs12".
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    Demo.apk
    bin/classes.dex
    bin/resources
    my-release-key.jks

nothing added to commit but untracked files present (use "git add" to track)
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
使用 zipalign 对齐未签署的 APK
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ zipalign -v -p 4 Demo.apk Demo-aligned.apk
Verifying alignment of Demo-aligned.apk (4)...
      53 AndroidManifest.xml (OK - compressed)
     708 res/drawable-hdpi-v4/ic_launcher.png (OK)
    8652 res/drawable-ldpi-v4/ic_launcher.png (OK)
   11116 res/drawable-mdpi-v4/ic_launcher.png (OK)
   15692 res/drawable-xhdpi-v4/ic_launcher.png (OK)
   27886 res/layout/main.xml (OK - compressed)
   28260 resources.arsc (OK)
   29937 classes.dex (OK - compressed)
Verification succesful
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    Demo-aligned.apk
    Demo.apk
    bin/classes.dex
    bin/resources
    my-release-key.jks

nothing added to commit but untracked files present (use "git add" to track)
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
通过 apksigner 使用私钥签署 APK
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ apksigner sign --ks my-release-key.jks --out Demo-release.apk Demo-aligned.apk
Keystore password for signer #1:
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    Demo-aligned.apk
    Demo-release.apk
    Demo.apk
    bin/classes.dex
    bin/resources
    my-release-key.jks

nothing added to commit but untracked files present (use "git add" to track)
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
验证 APK 是否已签署
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ apksigner verify Demo-release.apk
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    Demo-aligned.apk
    Demo-release.apk
    Demo.apk
    bin/classes.dex
    bin/resources
    my-release-key.jks

nothing added to commit but untracked files present (use "git add" to track)
lanzhiwang@lanzhiwang-dev2:~/AndroidStutio/demo$

```


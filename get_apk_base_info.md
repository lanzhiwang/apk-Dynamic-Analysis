## 安卓基本信息

step1: APK解包
```bash
apktool d -f MobileAnJian3.2.9.apk -o ./out/ -p ./out/
```

step2: 获取邮箱
```bash
$ grep -r -woah "[0-9a-zA-Z]\+@[0-9a-zA-Z]\{2,10\}\.[0-9a-z]\{2,5\}" ./out/ | uniq | sort | uniq
benjchristensen@netflix.com
ftp@example.com
hzhilamp@163.com
javamail@sun.com
youxifengwo@163.com
$
```

step3: 获取电话号码
```bash
$ grep -r -woah "1[0-9]\{10\}" ./out/ | uniq | sort | uniq
15527411905
$ grep -r -woah "[1-9]\{1\}[0-9]\{10\}" ./out/ | uniq | sort | uniq
15527411905
42860783596
$
```

step4: 解析图标
```bash
$ grep -r "android:icon" ./out/
./out/res/menu/crop_image_menu.xml:    <item android:icon="@drawable/crop_image_menu_rotate_left" android:id="@id/crop_image_menu_rotate_left" android:visible="false" android:title="@string/crop_image_menu_rotate_left" app:showAsAction="ifRoom" />
./out/res/menu/crop_image_menu.xml:    <item android:icon="@drawable/crop_image_menu_rotate_right" android:id="@id/crop_image_menu_rotate_right" android:title="@string/crop_image_menu_rotate_right" app:showAsAction="ifRoom" />
./out/AndroidManifest.xml:    <application android:allowBackup="true" android:icon="@mipmap/logo" android:label="@string/app_my_name" android:name="com.cyjh.mobileanjian.application.BaseApplication" android:theme="@style/Theme.App">

# 获取图标声明
android:icon="@drawable/crop_image_menu_rotate_left"
android:icon="@drawable/crop_image_menu_rotate_right"
android:icon="@mipmap/logo"

# 查找图标文件位置
$ find ./out/res/ -name drawable* -type d
./out/res/drawable-ldrtl-xxxhdpi-v4
./out/res/drawable-xhdpi-v4
./out/res/drawable-v23
./out/res/drawable-xxxhdpi-v4
./out/res/drawable-ldrtl-xhdpi-v4
./out/res/drawable
./out/res/drawable-ldrtl-hdpi-v4
./out/res/drawable-ldrtl-mdpi-v4
./out/res/drawable-mdpi-v4
./out/res/drawable-ldpi-v4
./out/res/drawable-v21
./out/res/drawable-ldrtl-xxhdpi-v4
./out/res/drawable-hdpi-v4
./out/res/drawable-xxhdpi-v4

# 查找图标文件
$ find `find ./out/res/ -name drawable* -type d` -name crop_image_menu_rotate_left* -type f
./out/res/drawable-xhdpi-v4/crop_image_menu_rotate_left.png
./out/res/drawable-xxxhdpi-v4/crop_image_menu_rotate_left.png
./out/res/drawable-hdpi-v4/crop_image_menu_rotate_left.png
./out/res/drawable-xxhdpi-v4/crop_image_menu_rotate_left.png

$ find `find ./out/res/ -name drawable* -type d` -name crop_image_menu_rotate_right* -type f
./out/res/drawable-xhdpi-v4/crop_image_menu_rotate_right.png
./out/res/drawable-xxxhdpi-v4/crop_image_menu_rotate_right.png
./out/res/drawable-hdpi-v4/crop_image_menu_rotate_right.png
./out/res/drawable-xxhdpi-v4/crop_image_menu_rotate_right.png

$ find ./out/res/ -name mipmap* -type d
./out/res/mipmap-mdpi-v4
./out/res/mipmap-xhdpi-v4
./out/res/mipmap-xxhdpi-v4
./out/res/mipmap-hdpi-v4

$ find `find ./out/res/ -name mipmap* -type d` -name logo* -type f
./out/res/mipmap-mdpi-v4/logo.png
./out/res/mipmap-xhdpi-v4/logo.png
./out/res/mipmap-xxhdpi-v4/logo.png
./out/res/mipmap-hdpi-v4/logo.png
```

step5: 获取URL链接
```bash
grep -r -wioah "http://[0-9_a-zA-Z\/.\-]\+" ./out/ | uniq | sort | uniq
```




# 图像篡改监测 C++ 
本文档包含了调用该方法所需的`OpenCV`安卓库、CMakeLists的配置，以及调用方法所需的步骤。

## OpenCV 安卓库的下载和配置
该库可以从 [OpenCV官网](https://opencv.org/releases/) 下载。请选择Android版本。

下载完成后，可以根据这些链接进行配置（[Youtube](https://www.youtube.com/watch?v=Sn3YhfY5jqg&t=2430s), 
[Medium](https://medium.com/android-news/a-beginners-guide-to-setting-up-opencv-android-library-on-android-studio-19794e220f3c),
[CSDN](https://blog.csdn.net/qq_37857689/article/details/126548915?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169258150516800184146654%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169258150516800184146654&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-1-126548915-null-null.142^v93^chatsearchT3_1&utm_term=opencv%20android%20native&spm=1018.2226.3001.4187)）。

配置完成后，请确认在 `cpp/includes/` 路径下有 `CMakeLists.txt`，`ela-dectect.h`，`ela-dectect.cpp`，和`native-lib.cpp` 文件，

并且在 `CMakeLists.txt` 中，正确配置了：
```
set(OpenCV_STATIC on)
set(OpenCV_DIR /the/path/to/your/OpenCV-android-sdk/sdk/native/jni)
find_package(OpenCV REQUIRED)

add_library(
        nativeopencv

        SHARED

        ela-detect.cpp
        native-lib.cpp

        #
        # 其他所拥有的配置
        #
)

find_library(jnigraphics-lib jnigraphics)

target_link_libraries( # Specifies the target library.
        nativeopencv

        ${OpenCV_LIBS}
        ${jnigraphics-lib}
        ${log-lib})

        #
        # 其他所拥有的配置
        #
)
```
最后，在 IDE 中 Make Project。

## 调用方法
在需要调用该方法的 Java 文件中，请添加：
```
static {
        // 加载库
        System.loadLibrary("nativeopencv");
}

public native boolean isForged(Bitmap bitmap, String path);
```
这个方法需要两个参数。

我们将图像以 `Bitmap` 的形式读取. 你可以通过以下代码将图片转换为 `Bitmap` ：
```
Bitmap src = BitmapFactory.decodeResource(getResources(),R.drawable.your_image_name);
```
我们的算法需要一个路径来保存处理过的图片。获取路径的代码：
```
File directory = this.getFilesDir();
String path = directory.getAbsolutePath();
```
现在你就可以调用方法 `isForged`，该方法会返回一个 `boolean` 值。如果我们的算法计算出图片是经过篡改的，会返回 `true`，否则将返回 `false`。

<br></br>
# Image Manipulation Detection for C++
This document contains the steps for configuring the native OpenCV library for Android, CMakeLists, and making native calls from Android Java Code.

## Download and Configure the OpenCV Library for Android
You can download the library from [OpenCV official webset](https://opencv.org/releases/). Please download the Android version.
After the download, you can follow any of the links ([Youtube](https://www.youtube.com/watch?v=Sn3YhfY5jqg&t=2430s), 
[Medium](https://medium.com/android-news/a-beginners-guide-to-setting-up-opencv-android-library-on-android-studio-19794e220f3c),
[CSDN](https://blog.csdn.net/qq_37857689/article/details/126548915?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169258150516800184146654%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169258150516800184146654&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-1-126548915-null-null.142^v93^chatsearchT3_1&utm_term=opencv%20android%20native&spm=1018.2226.3001.4187)) 
to complete the configuration.

Once you have done the configuration, please make sure you have `CMakeLists.txt`, `ela-dectect.h`, `ela-dectect.cpp`, and `native-lib.cpp` under your `cpp/includes/` folder.

Also, you need to make sure these lines have been added to your `CMakeLists.txt`.
```
set(OpenCV_STATIC on)
set(OpenCV_DIR /the/path/to/your/OpenCV-android-sdk/sdk/native/jni)
find_package(OpenCV REQUIRED)

add_library(
        nativeopencv

        SHARED

        ela-detect.cpp
        native-lib.cpp

        #
        # Other stuff you have
        #
)

find_library(jnigraphics-lib jnigraphics)

target_link_libraries( # Specifies the target library.
        nativeopencv

        ${OpenCV_LIBS}
        ${jnigraphics-lib}
        ${log-lib})

        #
        # Other stuff you have
        #
)
```

Once finishing your configuration, please remember to Make Project.

## Calling the native function
In the Java class where you want to call the native function. Please make sure you have
```
static {
        System.loadLibrary("nativeopencv");
}

public native boolean isForged(Bitmap bitmap, String path);
```
To call our method, you need two parameters. 

The image is read in `Bitmap`. Code example:
```
Bitmap src = BitmapFactory.decodeResource(getResources(),R.drawable.your_image_name);
```
Also, the `path` is REQUIRED for our algorithm. Please make sure you create a path and pass it to the method. Code example:
```
File directory = this.getFilesDir();
String path = directory.getAbsolutePath();
```
Now you are ready to call `isForged`, which will return a `boolean` value. If the image is forged, then `true` is returned, otherwise `false`.


- **源码https://github.com/geometer/FBReaderJ**

项目是将FBReaderJ转移至Android Studio上示例，代码没有CPP部分 - 发现代码较多，且现在AS都用CMake编译了，所以直接拷贝了`.so`在工程里，下面是我在Linux平台直接用ANT编译的笔记

FBReaderJ用Ant编译步骤
---

- 下载源码
git clone https://github.com/geometer/FBReaderJ.git

- 创建local.properties
添加内容
```
sdk.dir=/xx/xx/Android/Sdk
ndk.dir=/xx/xx/Android/Sdk/ndk-bundle
```

- 编译
    - `ant release`
    - 错误1：FBReaderJ/third-party/SuperToasts/build.xml:55: sdk.dir is missing. Make sure to generate local.properties using 'android update project' or to inject it through the ANDROID_HOME environment variable.
    ```
    cp local.properties third-party/SuperToasts/
    cp local.properties third-party/android-filechooser/
    cp local.properties third-party/android-filechooser/code/
    cp local.properties third-party/drag-sort-listview/library/
    cp local.properties third-party/AmbilWarna/
    ```
    - 错误2：Android/Sdk/tools/ant/build.xml:538: Unable to resolve project target 'android-14'
    将`third-party/drag-sort-listview/library/project.properties`、`third-party/AmbilWarna/project.properties`和`third-party/android-filechooser/code/project.properties`中的`android-xx`改为`android-19`
    - 错误3：import org.fbreader.util.NaturalOrderComparator;错误
    `mv fbreader/app/src/main/java/org/fbreader/util/NaturalOrderComparator.java third-party/android-filechooser/code/src/group/pals/android/lib/ui/filechooser/utils/`
    将`third-party/android-filechooser/code/src/group/pals/android/lib/ui/filechooser/utils/NaturalOrderComparator.java`、`fbreader/app/src/main/java/org/geometerplus/fbreader/library/FileTree.java`和`fbreader/app/src/main/java/org/geometerplus/fbreader/sort/TitledEntity.java`change the line:`import org.fbreader.util.NaturalOrderComparator;`to`import group.pals.android.lib.ui.filechooser.utils.NaturalOrderComparator;`
    注释third-party/android-filechooser/code/src/group/pals/android/lib/ui/filechooser/utils/FileComparator.java里的导包`//import org.fbreader.util.NaturalOrderComparator;`
    - 错误4：third-party/drag-sort-listview/library/src/com/mobeta/android/dslv/DragSortCursorAdapter.java:10: 错误: 程序包android.support.v4.widget不存在
    `cp libs/android-support-v4.jar third-party/drag-sort-listview/library/libs`

Tts-plus:https://github.com/gregko/TtsPlus-FBReader

参考：

http://www.goldyliang.net/blog/how-to-build-fbreaderj-android/

```
Copyright 2016 xuie0000

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
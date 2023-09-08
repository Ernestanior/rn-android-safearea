# rn-android-safearea
React Native android safeArea and gesture guide bar(手势指示器) white background problem solution


前言
最近在写 React Native 项目，调试应用时发现顶部状态栏和底部全面屏手势指示条区域不是透明的，看起来很难受。研究了一下这个问题，现在总结一下解决方案。

相关知识点：
React Native 原生组件 <StatusBar />
React Native 提供的 Hooks - useColorScheme
重写应用 Main Activity 的 onCreate 生命周期方法
修改 styles.xml 配置文件
顶部状态栏
顶部的状态栏可以使用 React Native 提供的 <StatusBar /> 组件实现透明

import { View, StatusBar, useColorScheme } from "react-native";
import type { FC } from "react";

const App: FC = () => {
  const colorScheme = useColorScheme();
  return (
    <View>
      <StatusBar
        translucent={true}
        backgroundColor="rgba(0,0,0,0)"
        barStyle={colorScheme === "dark" ? "light-content" : "dark-content"} // 设置文字颜色
      />
    </View>
  );
};

export default App;

底部导航栏
打开 /android/app/src/main/java/包名/MainActivity.java

在 MainActivity.java 中的 MainActivity 类中实现重写 onCreate 方法

@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  WindowCompat.setDecorFitsSystemWindows(getWindow(), false);
}

注意：onCreate 方法应该被写在 public class MainActivity extends ReactActivity 的内部

同时，在MainActivity.java 的头部 import 相关类

import android.os.Bundle;
import androidx.core.view.WindowCompat;

打开 /android/app/src/main/res/values/styles.xml
向 styles.xml 中添加内容

<item name="android:navigationBarColor">@android:color/transparent</item>

# wifi-direct-android
android wifi-direct  socket 

# 是什么
它可以不通过网络，也不通过蓝牙，只要两台设备都支持WiFi直连，打开WiFi，不用连接任何WiFi，就可以进行信息的传输（请忽略下面两张图中的WiFi连接标志，因为其与WiFi的连接与否无关，打开就可以）。
在Android的设置->网络与互联网->WLAN->WLAN偏好设置->高级->WLAN直连中可以找到关于Wi-Fi直连的设置


# 使用

- 在项目(最外层)的build.gradle 下面添加仓库地址
> allprojects {
    repositories {
        google()
        jcenter()
        maven { url "https://github.com/oceanhub168pub/maven_repo/raw/master" }  // 这一句
    }
}

- 在项目中引用
> api 'com.hub168.yh:wifi-direct-android:1.0.0@aar'

- 使用（分为客户端和服务端）

服务器端：

```
初始化:
        DirectServer directServer = YhServiceFactory.builder().create(getActivity(), this);
        directServer.startServer(); 
        
  public void onDestroy() {
    if (directServer != null) {
            directServer.stopServer();
            directServer.release(getActivity());
        }
  }
```

  
客户端：
```
   继承ClientCallback,监听wifi perr-perr 的状态变化
  
   @interface
   void onConnectedStatedChanged(int status, String address)
   {
        // status==1,address 不为空,表示连接上设备
   }

 
   DirectClient  directClient= YhClientFactory.builder().create(getActivity(), this);
   mClient.connected(mMacAddress);
  
   @Override
    public void onDestroy() {
        if (directClient != null) {
            directClient.disconnected();
            directClient.release(getActivity());
        }
        viewModel.close();
    }
```

> 注意： 客户端和服务器端在扫描连接之前都需要先打开 wifi开关,不需要联网

总结：

- 支持一对多设备连接

- 会出现扫描找不到设备

- 会有连接过程中断开的几率






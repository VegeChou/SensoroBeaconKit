Sensoro Beacon Kit 使用指南（Android）
==

##集成 SDK

####引入 jar 包

首先需要导入 jar 包

>sensorobeaconkit.jar

在工程根目录新建 libs 文件夹，将 jar 包拷贝到该文件夹中
####修改 AndroidManifest.xml

在 AndroidManifest.xml 文件加入 SBK 的依赖权限

```
<manifest
    ...
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
    ...
</manifest>
```

在 AndroidManifest.xml 文件加入 SBK 依赖的服务

```
<manifest
    ...
    <application
        ...
        <service android:name="com.sensoro.beacon.kit.BeaconProcessService"></service>
        <service android:name="com.sensoro.beacon.kit.BeaconService"></service>
        ...
    </application>
    ...
</manifest>
```

您可以从以下地址手动下载最新版本的 Android SDK：

[点击下载](https://raw.githubusercontent.com/VegeChou/SensoroBeaconKit/master/sensorobeaconkit.jar)

解压缩下载的文件，将 sensorobeaconkit.jar 按照上述步骤集成即可，最后结果如下

##开始使用 SDK

####获取 SensoroBeaconManager 对象

Sensoro Beacon Kit 中（以下简称 SBK），SensoroBeaconManager 是您的使用起点，SensoroBeaconManager 才用了单例设计模式，您应该这样取得 SensoroBeaconManager 的单例

```
SensoroBeaconManager beaconManager = SensoroBeaconManager.getInstance(context);
```

####实现 BeaconManagerListener 接口

您可以通过实现 BeaconManagerListener 接口来检测 Beacon 的出现,显示以及更新。

样例代码如下
```
BeaconManagerListener beaconManagerListener = new BeaconManagerListener() {

    @Override
    public void onUpdateBeacon(ArrayList<Beacon> beacons) {
        // Beacon 信息更新                  
    }
    
    @Override
    public void onNewBeacon(Beacon beacon) {
        // 发现一个新的 Beacon        
    }
    
    @Override
    public void onGoneBeacon(Beacon beacon) {
        // 一个 Beacon 消失     
    }
};
```

####启动 SBK

```
beaconManager.setBeaconManagerListener(beaconManagerListener);
beaconManager.startService();
```

当启动 SBK 后，您就可以发现设备周围的 Beacon 了。

**提示：请确保您的设备的蓝牙已经打开。**

####连接 Beacon

我们推荐 Beacon 的连接操作在UI主线程中进行.因为部分 Android 手机由于系统或者驱动的存在 BUG.

```
SensoroBeaconConnection beaconConnection = new SensoroBeaconConnection(this, beacon, new BeaconConnectionCallback() {
    @Override
    public void onConnectedState(Beacon beacon, int newState, int status) { }
    
    @Override
    public void onSetNewPassword(Beacon beacon, int status) { }
    
    @Override
    public void onDisabledPassword(Beacon beacon, int status) { }
    
    @Override
    public void onCheckPassword(Beacon beacon, int status) { }
    
    @Override
    public void onSetBaseSetting(Beacon beacon, int status) { }
    
    @Override
    public void onSetSensorSetting(Beacon beacon, int status) { }
    
    @Override
    public void onSetMajoMinor(Beacon beacon, int status) { }
    
    @Override
    public void onSetProximityUUID(Beacon beacon, int status) { }
    
    @Override
    public void onResetToFactory(Beacon beacon, int status) { }
    
    @Override
    public void onResetAcceleratorCount(Beacon beacon, int status) { }
    
    @Override
    public void onUpdateSensorData(Beacon beacon, int status) { }
    
    @Override
    public void onTemperatureDataUpdate(Beacon beacon, int temperature) { }
    
    @Override
    public void onBrightnessLuxDataUpdate(Beacon beacon, double lux) { }
    
    @Override
    public void onAcceleratorMovingUpdate(Beacon beacon, int isMovingState) { }
    
    @Override
    public void onAcceleratorCountUpdate(Beacon beacon, int count) { }
});

beaconConnection.connect();
```
####配置 Beacon

连接 Beacon 成功后,使用 SensoroBeaconConnection 实例对 Beacon 进行配置,具体的配置请参考 [doc](https://raw.githubusercontent.com/VegeChou/SensoroBeaconKit/master/javadoc/index.html) 说明文档 SensoroBeaconConnection 类说明。

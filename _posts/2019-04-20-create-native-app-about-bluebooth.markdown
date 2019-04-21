---
layout: post
title: "A app about Bluebooth"
subtitle: "How to connect the Bluebooth by a RN app and solve these problem"
date: 2019-04-20 22:00:00
author: "Vinter"
catalog: true
tags:
    - React Native
    - Bluetooth
---

> 新的知识，只有自己走过才知道问题的所在，经验就是在这样不断尝试和解决问题的过程积累的。       
> 路在自己的脚下，多走多想，总能看到远方。


好快，又一个多月过去，每次写自己的博客才发现，时间过得挺快，其实我大部分时间在[简书](https://www.jianshu.com/u/020b396f86e2)上写文章，没事写写想想，会发现很多新知识同时完善自己的知识体系，哈哈，IT的道路就是这样，打下强硬基石的前提下更要向新知识问好。有时候心情有点小烦躁的时候，写写文章，让自己的情感宣泄在这文章的海洋中，也算一种好的方式，有点能体会那些文人墨客的思想了，可能就是在经历过一些事情亦或者在情感受到事情的触发，交错复杂的时候才奋笔疾书的吧，哈哈！写着写着又扯远了，最近身边的事情有点多，所以脱离的主题。
好了，回归主题，今天主要讲的是如何使用RN做出的原生app连接蓝牙，并且和蓝牙通信传输数据，说到底就是踩坑之路，接下来让我慢慢道来。。。
<br/>

上章讲过了React Native，这里就不多说了，如何快速创建一个简单应用，请移步[RN官网](https://reactnative.cn/)。

## 蓝牙库
目前github上star最多的蓝牙通信库为[react-native-ble-manager](https://github.com/innoveit/react-native-ble-manager)和[react-native-ble-plx](https://github.com/innoveit/react-native-ble-manager)，react-native-ble-manager文档容易阅读，容易上手点，react-native-ble-plx有比较完善的文档，但是阅读理解需要花费点时间，现在github的star已经超越react-native-ble-manager，达到1.2k以上；相同的是，这两个库都是连接低功耗蓝牙的，具体什么是低功耗蓝牙，这里不多讲了，请自己找度娘，如果你的项目不是和低功耗蓝牙通信，就不要考虑了。

各位可以根据自己的需求选择这两个库，本人选择的是react-native-ble-manager，当然另一个库也测试了，没什么不同，主要调用的api不同而已。接下来进入主题。

## react-native-ble-manager
蓝牙数据通信主要有以下步骤：1.实例化蓝牙对象、2.初始化并监听蓝牙各状态、3.开启蓝牙、4.扫描设备、5.连接设备、6.获取Service和Characteristic，7.开启通知通道、8.读写数据、9.数据处理、10.断开连接。

这里主要讲6、7、8、9这四个步骤，其他步骤，请查看[react-native-ble-manager](https://github.com/innoveit/react-native-ble-manager)。


## 获取Service和Characteristic

蓝牙连接，android使用Mac地址与蓝牙连接，ios使用UUID与蓝牙连接。连接后就会获得设备的具体信息，返回数组，数组的每个对象中，主要分三部分：properties、characteristic、service；
<br/>
<br/>
**characteristic和service**
<br/>
service(服务)和characteristic(特征)都有一个128bit二进制的UUID作为唯一标志，8位二进制相当于2位十六进制，128bit二进制相当于32bit十六进制，所以十六进制UUID，如：0x0000xxxx-0000-1000-8000-00805F9B34FB，其中0x代表十六进制，实际中并没有，如：aa83b03e-8535-b5a0-7140-a304d2495cb7，这两个UUID用来连接蓝牙设备的通道，读写或通知；在实际中的过程中：你可能看到service(服务)和characteristic(特征)只有4bit十六进制，如：service:'1801' 或者characteristic：'2a03'等，这表示需要根据蓝牙技术联盟定义的UUID进行转化，
<br/>
这里需要讲解下两种UUID：一种是基本的UUID，就是上面所说的128bit的；另一种是代替基本UUID的16bit的UUID；
<br/>
蓝牙技术联盟定义UUID共用了一个基本的UUID:0x0000xxxx-0000-1000-8000-00805F9B34FB，所以如果你的4bit十六进制的是："1801"，那么它完整的UUID为0x00001801-0000-1000-8000-00805F9B34FB。
<br/>
<br/>
**properties**
<br/>
其中properties属性主要包括：通知通道（Notify）、读通道（Read）、写通道（Write或WriteWithoutResponse），然后调用retrieveServices方法获取Notify、Read、Write的serviceUUID和characteristicUUID作为参数来跟蓝牙进一步通信。


## 开启通知通道
蓝牙通信在使用读写通道时，可能需要调用通知通道来获取对应的消息，例如返回成功、失败或者是读到信息的字节流。当然有些情况是不需要，直接调用例如读通道，就会直接返回对应的字节流信息，这个就具体看你的实际项目情况了。
<br/>
这里讲解一下，该库返回的字节流均是10进制的字节流，例如[170,1,20,32,45,4,0,0]，然后根据你的具体需要进行转化，例如转化为十六进制，num.toString(16)，然后倒叙等等，本人项目中数据处理有点复杂，不仅数据按照了crc16进行加密了，而且排序方式按照低位在前，高位在后等，这文章后面再说。

## 读写数据
读写数据可以直接调用Write通道，即按照对应命令格式进行读写，也可以使用Read来进行读，这两种情况我都遇到过，不过总体来说还是通过Read来读数据简单方便，不用打开通知通道，直接promise返回对应的字节流信息。使用Write或者WriteWithoutResponse来读信息，需要打开通知通道来接受数据。当然这个看不同设备或者项目的要求了，这里不再多讲了。
<br/>
<br/>
接下来讲讲这个库写数据的大坑，该库写数据（Write）传参时只接受十六进制的字节流的数组，JS目前没办法把十进制字节流转化成十六进制的字节流，当初这个问题把我困扰了比较久，后来想到node的Buffer，用它来存储字节流，但是字节流方式还是十进制的，传入还是出现问题；想到这里没办法了，既然JS不支持十六进制的字节流，那就传入十六进制的字符串，这个就只能改JAVA的源码，由于我对JAVA不是很熟，这里请参照[幻影的博客](https://blog.csdn.net/withings/article/details/71378562)，文章说需要修改的源文件只有一个：在react-native-ble-manager\android\src\main\java\it\innove目录下的BleManager.java文件。
附上代码
```
    /** 16进制字符串转换成16进制byte数组，每两位转换 */
    public static byte[] strToHexByteArray(String str){
        byte[] hexByte = new byte[str.length()/2];
        for(int i = 0,j = 0; i < str.length(); i = i + 2,j++){
            hexByte[j] = (byte)Integer.parseInt(str.substring(i,i+2), 16);
        }
        return hexByte;
    }

    @ReactMethod
    public void write(String deviceUUID, String serviceUUID, String characteristicUUID, String message, Integer maxByteSize, Callback callback) {
        Log.d(LOG_TAG, "Write to: " + deviceUUID);

        Peripheral peripheral = peripherals.get(deviceUUID);
        if (peripheral != null) {
            // byte[] decoded = new byte[message.size()];
            // for (int i = 0; i < message.size(); i++) {
            //  decoded[i] = new Integer(message.getInt(i)).byteValue();
            // }
            // Log.d(LOG_TAG, "Message(" + decoded.length + "): " + bytesToHex(decoded));

            //message由原来的ReadableArray类型改为String类型，再将16进制字符串转化成16进制byte[]数组
            byte [] decoded = strToHexByteArray(message);
            Log.d(LOG_TAG, "decoded: " + Arrays.toString(decoded));         
            peripheral.write(UUIDHelper.uuidFromString(serviceUUID), UUIDHelper.uuidFromString(characteristicUUID), decoded, maxByteSize, null, callback, BluetoothGattCharacteristic.WRITE_TYPE_DEFAULT);
        } else
            callback.invoke("Peripheral not found");
    }

    @ReactMethod
    public void writeWithoutResponse(String deviceUUID, String serviceUUID, String characteristicUUID, String message, Integer maxByteSize, Integer queueSleepTime, Callback callback) {
        Log.d(LOG_TAG, "Write without response to: " + deviceUUID);

        Peripheral peripheral = peripherals.get(deviceUUID);
        if (peripheral != null) {
            // byte[] decoded = new byte[message.size()];
            // for (int i = 0; i < message.size(); i++) {
            //  decoded[i] = new Integer(message.getInt(i)).byteValue();
            // }
            // Log.d(LOG_TAG, "Message(" + decoded.length + "): " + bytesToHex(decoded));

            //message由原来的ReadableArray类型改为String类型，再将16进制字符串转化成16进制byte[]数组
            byte [] decoded = strToHexByteArray(message);
            Log.d(LOG_TAG, "decoded: " + Arrays.toString(decoded));         
            peripheral.write(UUIDHelper.uuidFromString(serviceUUID), UUIDHelper.uuidFromString(characteristicUUID), decoded, maxByteSize, queueSleepTime, callback, BluetoothGattCharacteristic.WRITE_TYPE_NO_RESPONSE);
        } else
            callback.invoke("Peripheral not found");
    }
```
![image](https://upload-images.jianshu.io/upload_images/5097943-a5bf6255007154ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "image")
![image](https://upload-images.jianshu.io/upload_images/5097943-ea174a24cbf00ec3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "image")

按照文章说的，做了这些修改，开心的以为要成功，但还是报错了，因为没有引入工具包，这里幻影博客没有讲清楚，这里我对其进行补充，代码如下
```
import java.lang.reflect.Method;
import java.util.Iterator;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.*;  //  添加
```
导入java中util包的所有类库



## 数据处理
前面讲了修改java代码支持传入十六进制字符串，但是根据不同的设备传入的数据格式要求是不一样的，但是crc校验基本都是必要的，即在字节流加入一段规定位数的crc校验码，这里简单讲一下crc校验的作用和排序方式。
<br/>
<br/>
**CRC校验**
<br/>
数据通信领域中最常用的一种查错校验码，它根据不同的多项式对数据进行计算，从而得到对应的校验码，接受设备中也执行对应的多项式运算，从而验证数据的可靠性。这就防止了因传输过程的数据丢失，传来的错误数据。这里介绍一个很好的[CRC](https://www.npmjs.com/package/crc)的npm包，里面包含了很多种多项式计算方式。
<br/>
不要以为这样就解决了，本人以为CRC验证传入十六进制的字符串就可以了，因为从网上在线CRC校验就是这样传入的，万万没想到得到的数据和真实的CRC校验的到的结果不一样，什么鬼，后来还是回到字节流的问题上，这个是因为该CRC库需要传入字节流，后来用了node的Buffer来存储字节流传入，到了真实的结果。
<br/>
<br/>
**排序方式**
<br/>
十六进制字节流可以按两种方式排序，一种是常规排序，高位在前，低位在后；另一种就是低位在前，高位在后，例如一个2字节（4bit）数字如：b26c，按照低位在前，高位在后就是6cb2。这种方式能够更好的保护数据，就算数据被窃取，也不一定能够转化出来，不过这需要对数据进行各种倒叙转换等，因为一串字节流中，数据位数可能是比较长的。
<br/>
以上两点只是数据流中的一部分，具体的项目要求的格式是不同的，每一帧（字节）代表的意思是不同的，按照规定的格式写数据，就需要按照规定的格式解析数据。
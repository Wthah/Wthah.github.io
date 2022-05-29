---
title: SIP服务接口
date: 2022/5/13 11:21:25
tags:
  - WVP
categories:
  - 视频监控
cover: /img/api/api.jpg
---



# SIP服务接口

## 1. 点播

![image-20220512145958358](/img/image-20220512145958358.png)

> http://localhost:9970/api/play/start/44010200492000000001/34020000001320000001

```json
{
    "code": 0,
    "msg": "success",
    "data": {
        "app": "rtp",
        "stream": "44010200492000000001_34020000001320000001",
        "deviceID": "44010200492000000001",
        "channelId": "34020000001320000001",
        "flv": "http://192.168.3.197:9980/rtp/44010200492000000001_34020000001320000001.live.flv",
        "https_flv": "https://192.168.3.197:443/rtp/44010200492000000001_34020000001320000001.live.flv",
        "ws_flv": "ws://192.168.3.197:9980/rtp/44010200492000000001_34020000001320000001.live.flv",
        "wss_flv": "wss://192.168.3.197:443/rtp/44010200492000000001_34020000001320000001.live.flv",
        "fmp4": "http://192.168.3.197:9980/rtp/44010200492000000001_34020000001320000001.live.mp4",
        "https_fmp4": "https://192.168.3.197:443/rtp/44010200492000000001_34020000001320000001.live.mp4",
        "ws_fmp4": "ws://192.168.3.197:9980/rtp/44010200492000000001_34020000001320000001.live.mp4",
        "wss_fmp4": "wss://192.168.3.197:443/rtp/44010200492000000001_34020000001320000001.live.mp4",
        "hls": "http://192.168.3.197:9980/rtp/44010200492000000001_34020000001320000001/hls.m3u8",
        "https_hls": "https://192.168.3.197:443/rtp/44010200492000000001_34020000001320000001/hls.m3u8",
        "ws_hls": "ws://192.168.3.197:9980/rtp/44010200492000000001_34020000001320000001/hls.m3u8",
        "wss_hls": "wss://192.168.3.197:443/rtp/44010200492000000001_34020000001320000001/hls.m3u8",
        "ts": "http://192.168.3.197:9980/rtp/44010200492000000001_34020000001320000001.live.ts",
        "https_ts": "https://192.168.3.197:443/rtp/44010200492000000001_34020000001320000001.live.ts",
        "ws_ts": "ws://192.168.3.197:9980/rtp/44010200492000000001_34020000001320000001.live.ts",
        "wss_ts": "wss://192.168.3.197:443/rtp/44010200492000000001_34020000001320000001.live.ts",
        "rtmp": "rtmp://192.168.3.197:1935/rtp/44010200492000000001_34020000001320000001",
        "rtmps": "rtmps://192.168.3.197:19350/rtp/44010200492000000001_34020000001320000001",
        "rtsp": "rtsp://192.168.3.197:554/rtp/44010200492000000001_34020000001320000001",
        "rtsps": "rtsps://192.168.3.197:332/rtp/44010200492000000001_34020000001320000001",
        "rtc": "https://192.168.3.197:443/index/api/webrtc?app=rtp&stream=44010200492000000001_34020000001320000001&type=play",
        "mediaServerId": "FQ3TF8yT83wh5Wvz",
        "tracks": [
            {
                "codecIdName": null,
                "codecId": 0,
                "channels": 0,
                "sampleBit": 0,
                "ready": true,
                "fps": 25,
                "width": 1920,
                "codecType": 0,
                "sampleRate": 0,
                "height": 1080
            },
            {
                "codecIdName": null,
                "codecId": 0,
                "channels": 1,
                "sampleBit": 0,
                "ready": true,
                "fps": 0,
                "width": 0,
                "codecType": 0,
                "sampleRate": 0,
                "height": 0
            }
        ],
        "startTime": null,
        "endTime": null,
        "progress": 0.0,
        "transactionInfo": null
    }
}
```

###### 停止点播

![image-20220512160420712](/img/image-20220512160420712.png)

> url: http://localhost:9970/api/play/stop/44010200492000000001/34020000001320000001

```json
{"deviceId":"44010200492000000001","channelId":"34020000001320000001"}
```

## 2. 控制摄像头移动

![image-20220512152712102](/img/image-20220512152712102.png)

> url: http://localhost:9970/api/ptz/control/44010200492000000001/34020000001320000001?command=stop&horizonSpeed=255&verticalSpeed=255&zoomSpeed=255

![image-20220512152923243](/img/image-20220512152923243.png)

## 3. 查询设备

###### 分页查询

![image-20220512153106334](/img/image-20220512153106334.png)

> url: http://localhost:9970/api/device/query/devices?page=1&count=15

```json
{
    "total": 1,
    "list": [
        {
            "deviceId": "44010200492000000001",
            "name": "IP CAMERA",
            "manufacturer": "Hikvision",
            "model": "DS-IPC-T12V3-I",
            "firmware": "V5.5.800",
            "transport": "TCP",
            "streamMode": "UDP",
            "ip": "192.168.3.102",
            "port": 52069,
            "hostAddress": "192.168.3.102:52069",
            "online": 1,
            "registerTime": "2022-05-12 15:24:18",
            "keepaliveTime": "2022-05-12 15:29:23",
            "channelCount": 1,
            "expires": 3600,
            "createTime": "2022-05-11 17:42:59",
            "updateTime": "2022-05-12 15:29:23",
            "mediaServerId": null,
            "firsRegister": false,
            "charset": "GB2312",
            "subscribeCycleForCatalog": 0,
            "subscribeCycleForMobilePosition": 0,
            "mobilePositionSubmissionInterval": 5,
            "subscribeCycleForAlarm": 0,
            "ssrcCheck": true
        }
    ],
    "pageNum": 1,
    "pageSize": 15,
    "size": 1,
    "startRow": 1,
    "endRow": 1,
    "pages": 1,
    "prePage": 0,
    "nextPage": 0,
    "isFirstPage": true,
    "isLastPage": true,
    "hasPreviousPage": false,
    "hasNextPage": false,
    "navigatePages": 8,
    "navigatepageNums": [
        1
    ],
    "navigateFirstPage": 1,
    "navigateLastPage": 1
}
```

###### 根据id查询

![image-20220512153332457](/img/image-20220512153332457.png)

> url：http://localhost:9970/api/device/query/devices/44010200492000000001

```json
{
    "deviceId": "44010200492000000001",
    "name": "IP CAMERA",
    "manufacturer": "Hikvision",
    "model": "DS-IPC-T12V3-I",
    "firmware": "V5.5.800",
    "transport": "TCP",
    "streamMode": "UDP",
    "ip": "192.168.3.102",
    "port": 52069,
    "hostAddress": "192.168.3.102:52069",
    "online": 1,
    "registerTime": "2022-05-12 15:24:18",
    "keepaliveTime": "2022-05-12 15:34:24",
    "channelCount": 0,
    "expires": 3600,
    "createTime": "2022-05-11 17:42:59",
    "updateTime": "2022-05-12 15:34:24",
    "mediaServerId": null,
    "firsRegister": false,
    "charset": "GB2312",
    "subscribeCycleForCatalog": 0,
    "subscribeCycleForMobilePosition": 0,
    "mobilePositionSubmissionInterval": 5,
    "subscribeCycleForAlarm": 0,
    "ssrcCheck": true
}
```

## 4. 查询设备下对应的通道

![image-20220512154939704](/img/image-20220512154939704.png)

> url：http://localhost:9970/api/device/query/devices/44010200492000000001/channels?page=1&count=15

```json
{
    "total": 1,
    "list": [
        {
            "id": 81666,
            "channelId": "34020000001320000001",
            "deviceId": "44010200492000000001",
            "name": "Camera 01",
            "manufacture": "Hikvision",
            "model": "IP Camera",
            "owner": "Owner",
            "civilCode": "4401020049",
            "block": "",
            "address": "Address",
            "parental": 0,
            "parentId": "44010200492000000001",
            "safetyWay": 0,
            "registerWay": 1,
            "certNum": "",
            "certifiable": 0,
            "errCode": 0,
            "endTime": null,
            "secrecy": "0",
            "ipAddress": "",
            "port": 0,
            "password": "",
            "createTime": "",
            "updateTime": "2022-05-11 18:44:06",
            "status": 1,
            "longitude": 0.0,
            "latitude": 0.0,
            "subCount": 0,
            "streamId": null,
            "hasAudio": true,
            "channelType": 0,
            "ptztypeText": "未知",
            "ptztype": 0
        }
    ],
    "pageNum": 1,
    "pageSize": 15,
    "size": 1,
    "startRow": 1,
    "endRow": 1,
    "pages": 1,
    "prePage": 0,
    "nextPage": 0,
    "isFirstPage": true,
    "isLastPage": true,
    "hasPreviousPage": false,
    "hasNextPage": false,
    "navigatePages": 8,
    "navigatepageNums": [
        1
    ],
    "navigateFirstPage": 1,
    "navigateLastPage": 1
}
```

## 5. 移除设备

![image-20220512155135385](/img/image-20220512155135385.png)

> url：http://localhost:9970/api/device/query/devices/44010200492000000001/delete

```json
{"deviceId":"44010200492000000001"}
```

## 6. 设备状态查询

![image-20220512155612192](/img/image-20220512155612192.png)

> url: http://localhost:9970/api/device/query/devices/44010200492000000001/status


# React + Firebase + esp8266 实时物联网应用

> 原文：<https://medium.com/swlh/real-time-iot-app-with-react-firebase-esp8266-e2e673efd1bf>

在本教程中，我们将浏览一个 **react 应用**的创建，该应用显示来自**超声波传感器**的**实时**信息，该传感器连接到 **NodeMCU esp8266 板** (Wi-Fi 模块)，该板发送数据并将其存储在 **Firebase 数据库**中。

该项目测量到水箱中的水的距离，并通过 Wi-Fi 将信息发送到数据库，以便可以显示到水箱中液体的距离的网页。
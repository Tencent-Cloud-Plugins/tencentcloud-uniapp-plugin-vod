# 腾讯云云点播（VOD）插件

## 1. 插件介绍
| 标题      | 名称    |
| ----     | ---------------- |
| 中文名称   | 腾讯云云点播（VOD）插件 |
| 英文名称   | tencentcloud-plugin-vod |
| 最新版本   | v1.0.1 (2020.11.05) |
| 适用平台   | [DCloud uni-app](https://uniapp.dcloud.net.cn) |
| 适用产品   | [腾讯云云点播（VOD）](https://cloud.tencent.com/product/vod) |
| GitHub项目| [tencentcloud-uniapp-plugin-vod](https://github.com/Tencent-Cloud-Plugins/tencentcloud-uniapp-plugin-vod) |
| 主创团队   | 腾讯云中小企业产品中心（SMB Product Center of Tencent Cloud） |
| 兼容平台   | H5、小程序、APP |

一款帮助开发者在uni-app项目开发中快捷使用腾讯云云点播（VOD）产品功能的插件。

## 2. 功能特性

- 上传媒体文件需要的签名计算功能
- 通过媒体ID获取媒体详细信息的功能
- 生成媒体防盗链接的功能

## 3. 安装指引

本插件需要调用uniCloud云函数，而使用云函数的前提是：

- 使用DCloud官方开发工具HBuilderX 2.7+；
- 已注册DCloud开发者账号并通过实名认证；
- 开通了uniCloud并创建一个腾讯云的服务空间；

### 3.1. 新建或打开已有项目

1. 打开HBuilderX开发工具；
1. 新建或打开一个uni-app项目；

### 3.2. 导入云函数

1. 访问 DCloud 插件市场的 [腾讯云插件 - 云函数模板](https://ext.dcloud.net.cn/plugin?id=2139) 详情页；
2. 点击详情页右上角 **使用 HBuilderX 导入插件**，将云函数模板导入到您的项目中；
![](./images/guide/guide-1.png)
3. 在项目中打开 _cloudfunctions/tencentcloud-plugin/config.js_ 文件，将腾讯云的密钥信息配置进去，可以在腾讯云 [API 密钥管理](https://console.cloud.tencent.com/cam/capi) 中获取 SecretId、SecretKey 和 APPID；
![](./images/guide/guide-2.png)
4. 在[uniCloud控制台](https://unicloud.dcloud.net.cn/login)注册HBuild账号并登录，创建[云服务空间](https://uniapp.dcloud.net.cn/uniCloud/concepts/space)；
![](./images/guide/guide-3.png)
5. 绑定云函数的云服务空间，将[云函数](https://uniapp.dcloud.net.cn/uniCloud/concepts/cloudfunction) [**上传部署**](https://uniapp.dcloud.net.cn/uniCloud/quickstart?id=rundebug) 到您的[云服务空间](https://uniapp.dcloud.net.cn/uniCloud/concepts/space)；
![](./images/guide/guide-4.png)

> 如果您之前在使用其它腾讯云产品的 uni-app 插件时已经导入过此云函数模板，则前 4 个步骤可以省略。

> 若导入失败或有其它疑问，请查看 [uniCloud帮助文档](https://uniapp.dcloud.io/uniCloud/README) 云函数相关章节。

### 3.3. 导入插件

1. 访问DCloud插件市场 [腾讯云云点播（VOD）插件](https://ext.dcloud.net.cn/plugin?id=2363) 详情页；
1. 点击详情页右上角 **使用HBuilderX导入插件** ，将插件导入到您的项目中；
1. 在项目中通过 import 语法将插件提供的方法导入到相关业务组件并使用；

> 本插件的默认导入位置是在您项目的“js_sdk”目录下

## 4. 使用指引

### 4.1. 插件API导图

![](./images/vod-guide.png)

### 4.2. 插件使用示例

```javascript
// H5环境通过云函数计算签名上传媒体文件示例

import TcVod from 'vod-js-sdk-v6';
import getSignature from '@/js_sdk/tencentcloud-plugin-vod/get-upload-signature.js';
import chooseFile from '@/js_sdk/tencentcloud-plugin-cos/choose-file.js';

export default {
  methods: {
    async eventHandler() {
      const [video] = await chooseFile('video/*');
      const [image] = await chooseFile('image/*');
      const tcVod = new TcVod({ getSignature });
      const uploader = tcVod.upload({
        mediaFile: video,
        coverFile: image,
      });
      try {
        const result = await uploader.done();
        console.log(result);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
```

```javascript
// 通过媒体ID获取媒体详情的示例

import getMediaInfo from '@/js_sdk/tencentcloud-plugin-vod/get-media-info.js'

export default {
  methods: {
    async eventHandler() {
      try {
        const mediaID = '123456****1234';
        const result = await getMediaInfo(mediaId);
        console.log(result);
        // 如果在云函数层配置了key防盗链信息，则防盗链接会通过字段 result.BasicInfo.AntiTheftUrl 返回
        // 其它字段详情请查阅文档 https://cloud.tencent.com/document/api/266/31773#MediaInfo
      } catch (error) {
        console.log(error);
      }
    }
  }
};
```

### 4.3. 名词解释

服务空间：一个服务空间对应一整套独立的云开发资源，包括数据库、存储空间、云函数等资源。服务空间之间彼此隔离。更多详情请访问 [uniCloud开发文档](https://uniapp.dcloud.io/uniCloud/concepts/space)

云函数：云函数是运行在云端的JavaScript代码，更多详情请见 [uniCloud云函数文档](https://uniapp.dcloud.io/uniCloud/cf-functions)

## 5. 获取入口

| 插件入口      | 链接    |
| ----     | ---------------- |
| DCloud插件市场 | [腾讯云云点播（VOD）插件](https://ext.dcloud.net.cn/plugin?id=2363) |

## 6. FAQ
> 暂无


## 7. GitHub版本迭代记录

### 7.1. tencentcloud-uniapp-plugin-vod v1.0.1

- 优化代码

### 7.2. tencentcloud-uniapp-plugin-vod v1.0.0

- 新增客户端上传媒体文件需要的签名计算功能
- 新增客户端通过媒体ID获取媒体详细信息的功能
- 新增生成媒体防盗链接的功能

## 8. 联系我们

&nbsp;&nbsp;&nbsp;扫码备注“春雨”来联络到我们</br>
![](../images/qrcode.png)
---

copyright:
years: 2015, 2017

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}

# 使 Web 应用程序能够接收推送通知
{: #web_notifications}
上次更新时间：2017 年 4 月 12 日
{: .last-updated}

您可以启用 Google Chrome、Mozilla Firefox 和 Safari Web 应用程序以接收 {{site.data.keyword.mobilepushshort}}。请确保您已经完成[配置通知提供程序的凭证](t__main_push_config_provider.html)，然后再继续相关步骤。

## 安装 Web 浏览器客户机 SDK 以支持 {{site.data.keyword.mobilepushshort}} 
{: #web_install}

本主题描述如何安装和使用客户机 JavaScript 推送 SDK 来进一步开发 Web 应用程序。

### 在 Web 应用程序中初始化
{: #web_initialise_web_app}

要在 Google Chrome Web 应用程序中安装 JavaScript SDK，请完成以下步骤：

从 [Bluemix Web 推送 SDK](https://codeload.github.com/ibm-bluemix-mobile-services/bms-clientsdk-javascript-webpush/zip/master){: new_window} 下载 `BMSPushSDK.js`、`BMSPushServiceWorker.js` 和 `manifest_Website.json` 文件。

1. 编辑 `manifest_Website.json` 文件。
	- 对于 Google Chrome 浏览器，将 `name` 更改为站点名称。例如，`www.dailynewsupdates.com`。将 `gcm_sender_id` 更改为 Firebase 云消息传递 (FCM) 或 Google 云消息传递 (GCM) sender_ID。有关更多信息，请参阅[配置通知提供程序的凭证](t__main_push_config_provider.html)。gcm_sender_id 值仅包含数字。

		```
			{
	"name": "YOUR_WEBSITE_NAME",
  			"gcm_sender_id": "GCM_Sender_Id"
			 }
		```
    		{: codeblock}
 
	- 对于 Mozilla Firefox 浏览器，在 `manifest_Website.json` 文件中添加以下值。提供相应的`名称`。该名称应该是 Web 站点的名称。

		```
			{ 
	"name": "YOUR_WEBSITE_NAME"
			 }
		```
    		{: codeblock}

2. 将 `manifest_Website.json` 文件名更改为 `manifest.json`。
3. 将 `BMSPushSDK.js`、`BMSPushServiceWorker.js` 和 `manifest.json` 添加到 Web 站点的根目录。
3. 将 `manifest.json` 包含在 html 文件的 `<head>` 标记中。
	```
		<link rel="manifest" href="manifest.json">
	```
    	{: codeblock}
4. 将 Bluemix Web 推送 SDK 包含在您的 Web 应用程序中。
	```
		<script src="BMSPushSDK.js" async></script>
	```
    	{: codeblock}

**注**：请确保已部署代码且使用 `https` 而非 `http` 访问样本链接。 

## 初始化 Web 推送 SDK 
{: #web_initialize}

通过 Bluemix {{site.data.keyword.mobilepushshort}} 服务 `app GUID` 和 `app Region` 初始化推送 SDK。  

要获取您的应用程序 GUID，在初始化的推送服务的导航窗格中选择**配置**选项，并单击**移动选项**。修改代码片段以使用 Bluemix 推送通知服务 appGUID 参数。 

`App Region` 指定托管 {{site.data.keyword.mobilepushshort}} 服务的位置。可以使用以下三个值中的一个值：

 - 对于美国达拉斯：`.ng.bluemix.net`
 - 对于英国：`.eu-gb.bluemix.net`
 - 对于悉尼：`.au-syd.bluemix.net`

```
	    var bmsPush = new BMSPush();
    function callback(response) {
        alert(response.response)
    }
    var initParams = {
      "appGUID":"push app GUID",
  "appRegion":"Region where service hosted",
   "clientSecret":"clientSecret of your push service"
   "websitePushIDSafari": "Optional parameter for Safari Push Notifications only. The value should match the website Push ID provided during the server side configuration."
    }
  bmsPush.initialize(initParams, callback)
```
	{: codeblock}

**注**：如果针对 Web 推送 SDK 更改了您的 FCM 凭证，那么对于 Chrome 浏览器，消息传送可能会失败。请确保调用 `bmsPush.unRegisterDevice` 以避免失败。

如果您提供错误的参数，那么您可能会看到配置相关错误。有关更多信息，请参阅[故障诊断](troubleshooting.html)。

## 注册 Web 应用程序 
{: #web_register}

使用 **register()** API 向 {{site.data.keyword.mobilepushshort}} 服务注册设备。基于您的浏览器，使用以下任一选项。

- 对于从 Google Chrome 注册，请在 Bluemix {{site.data.keyword.mobilepushshort}} 服务 Web 配置仪表板中添加 Firebase 云消息传递 (FCM) 和 Google 云消息传递 (GCM) API 密钥和 Web 站点 URL。有关更多信息，请参阅 Chrome 设置下的[配置通知提供程序的凭证](t__main_push_config_provider.html)。

- 对于从 Mozilla Firefox 注册，在 Firefox 设置下的 Bluemix {{site.data.keyword.mobilepushshort}} 服务 Web 配置仪表板中添加 Web 站点 URL。

使用以下代码片段在 Bluemix {{site.data.keyword.mobilepushshort}} 服务中注册。

```
	    var bmsPush = new BMSPush();
    function callback(response) {
        alert(response.response)
    }
    var initParams = {
      "appGUID":"push app GUID",
  "appRegion":"Region where service hosted",
  "clientSecret":"clientSecret of your push service"
  "websitePushIDSafari": "Optional parameter for Safari Push Notifications only. The value should match the website Push ID provided during the server side configuration."
  }
  bmsPush.initialize(params, callback)
    bmsPush.register(function(response) {
      alert(response.response)
  })
```
    {: codeblock}


## 向 Web 浏览器发送基本通知
{: #web_browsers}

开发应用程序后，可以发送推送通知。 

1. 选择**发送通知**，并通过选择 **Web 通知**作为**发送至**选项来编辑消息。 
2. 键入需要在**消息**字段中传递的消息。
3. 您可以选择提供可选设置：
  - **通知标题**：这是作为消息警报标题显示的文本。
  - **通知图标 URL**：如果您的消息需要与应用程序通知图标一起传递，请在字段中提供图标的链接。
  - **生存时间**：通知服务器消息的有效性。
4. 对于发送到 Safari 浏览器的 Web 通知，需要一些附加信息：
  - **操作**：这是操作按钮的标签	。
  - **URL 自变量**：需要与此通知一起使用的 URL 自变量。请确保以 JSON 数组的形式提供。 
 
下面的图像显示仪表板中的 Web 通知选项。

  ![“通知”屏幕](images/DashboardWebpush.jpg)


### 后续步骤
{: #next_steps_tags}

成功设置基本通知后，可以选择配置基于标记的通知和高级选项。

将这些 {{site.data.keyword.mobilepushshort}} 服务功能添加到应用程序中。要使用基于标记的通知，请参阅[基于标记的通知](c_tag_basednotifications.html)。要使用高级通知选项，请参阅[高级通知](t_advance_badge_sound_payload.html)。







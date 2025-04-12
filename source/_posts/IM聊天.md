---
title: IM聊天
date: 2021-08-21 16:57:49
top_img: https://static001.geekbang.org/resource/image/88/f1/8807661ef5b82fcb75e8b8f2dbd71ef1.jpg
cover: https://static001.geekbang.org/resource/image/88/f1/8807661ef5b82fcb75e8b8f2dbd71ef1.jpg
tags:
- IM聊天
categories:
- IM聊天
aplayer: true
---
## IM聊天

耗时三个月终于在公司完成了一个聊天系统的需求，过程很麻烦，但是经过自己的不断探索还是成功开发出来并且完美上线。这算是自己接触的一个比较完整的利用前端API进行的开发，所以在这里也做一个总结。

IM聊天主要利用的是腾讯云的[即时通讯](https://cloud.tencent.com/document/product/269)进行消息功能实现。里面的`SDK`文档相当详细，也有不错的demo供开发者学习，能实现消息的发送、接收功能其实并不难和后端做好数据上的处理基本都可以实现，主要是公司的聊天系统需求比较复杂，一直在进行不断的迭代更新，包括后期实现的智能客服聊天系统、聊天远程控制功能等等。总体来说还是比较复杂，这里主要针对开发进行一个大致的总结，详细的可以在我的github看看[源代码](https://github.com/Aiolimp/IM-VisitorMessage)以作参考。

项目主要针对web端实现IM聊天，可以看看[官方文档](https://cloud.tencent.com/document/product/269/37411)提供的教程。

**聊天界面：**

客服端：

![在这里插入图片描述](https://img-blog.csdnimg.cn/55af7f5e2f03458caf85583696eeffe6.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

访客端：



### **初始化TIM**

接入前，您需要在 [云通信控制台](https://console.cloud.tencent.com/avc) 中创建一个云通信应用，并取得 `SDKAppID`。

| API                                                          | 描述            |
| :----------------------------------------------------------- | :-------------- |
| [create](https://web.sdk.qcloud.com/im/doc/zh-cn//TIM.html#.create) | 创建 SDK 实例。 |

```js
//src/utils/tim.js
import TIM from 'tim-js-sdk';
import COS from "cos-js-sdk-v5";


function tim (SDKAppID){
  let options = {
    SDKAppID: SDKAppID // 接入时需要将0替换为您的即时通信 IM 应用的 SDKAppID
  };
  // 创建 SDK 实例，`TIM.create()`方法对于同一个 `SDKAppID` 只会返回同一份实例
  let tim = TIM.create(options); // SDK 实例通常用 tim 表示
  
  // 设置 SDK 日志输出级别，详细分级请参见 setLogLevel 接口的说明
  tim.setLogLevel(1); // 普通级别，日志量较多，接入时建议使用
  
  // 注册 COS SDK 插件
  tim.registerPlugin({'cos-js-sdk': COS});
  return tim
}
export default tim
```

**在main.js中全局挂载：**

```js
import Vue from "vue";
import App from "./App.vue";
import store from "./store/index";
import {
  router
} from "./routes/routes";
import Antd from "ant-design-vue";
import './api/index'
import tim from './utils/tim'
import TIM from 'tim-js-sdk'

Vue.prototype.tim = tim
Vue.prototype.TIM = TIM
Vue.prototype.$bus = new Vue() // event Bus 用于无关系组件间的通信。
Vue.use(Antd);

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount("#app");
```

### 登录相关

1.首先初始化签名

使用 用户ID(userID) 和 签名串(userSig) 登录即时通信 IM，登录流程有若干个异步执行的步骤，使用返回的 Promise 对象处理登录成功或失败。

```js
 login() {
      let promise = tim.login({
        userID: "your userID",
        userSig: "your userSig",
      });
      promise
        .then(function (imResponse) {
          console.log(imResponse.data); // 登录成功
          if (imResponse.data.repeatLogin === true) {
            /
            console.log(imResponse.data.errorInfo);
          }
        })
        .catch(function (imError) {
          console.warn("login error:", imError); // 登录失败的相关信息
        });
    },
```

2.登录成功后会触发 SDK_READY 事件，该事件触发后，可正常使用 SDK 接口

```js
  initListener() {
      // 登录成功后会触发 SDK_READY 事件，该事件触发后，可正常使用 SDK 接口
      this.tim(this.infoObj.sdkAppID).on(
        this.TIM.EVENT.SDK_READY,
        this.onReadyStateUpdate,
        this
      );
      // SDK NOT READT
      this.tim(this.infoObj.sdkAppID).on(
        this.TIM.EVENT.SDK_NOT_READY,
        this.onReadyStateUpdate,
        this
      );
      // 被踢出
      this.tim(this.infoObj.sdkAppID).on(
        this.TIM.EVENT.KICKED_OUT,
        this.onKickOut
      );
      // SDK内部出错
      this.tim(this.infoObj.sdkAppID).on(this.TIM.EVENT.ERROR, this.onError);
      // 收到新消息
      this.tim(this.infoObj.sdkAppID).on(
        this.TIM.EVENT.MESSAGE_RECEIVED,
        this.onReceiveMessage
      );
      // 会话列表更新
      this.tim(this.infoObj.sdkAppID).on(
        this.TIM.EVENT.CONVERSATION_LIST_UPDATED,
        this.onUpdateConversationList,
        this
      );
    },
```

3.登出

```js
    logout(context) {
      // 若有当前会话，在退出登录时已读上报
      if (context.rootState.conversation.currentConversation.conversationID) {
        tim(store.state.basic.imInfo.sdkAppID).setMessageRead({ conversationID:               context.rootState.conversation.currentConversation.conversationID })
      }
      tim(store.state.basic.imInfo.sdkAppID).logout().then(() => {
        context.commit('toggleIsLogin')
        context.commit('stopComputeCurrent')
        context.commit('reset')
      })
    }
```

### 消息类型

消息类型主要分为自定义消息和普通消息。

![image-20210818161849499](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20210818161849499.png)

#### **自定义消息：**

创建自定义消息实例的接口，此接口返回一个消息实例，当 SDK 提供的能力不能满足您的需求时，可以使用自定义消息进行个性化定制。

自定义消息需要和后端沟通定义子消息类型。

```js
 //访客端发送自定义消息
    sendCustomMessage(data) {
      let data1 = JSON.stringify(data);
      if (this.sessionObj.serviceImAccount&&data1) {
        let message = this.tim(this.imInfo.sdkAppID).createCustomMessage({
          to: this.sessionObj.serviceImAccount,
          conversationType: this.currentConversationType
            ? this.currentConversationType
            : "C2C",
          payload: {
            data: data1,
            description: "",
            extension: "",
          },
        });
        this.tim(this.imInfo.sdkAppID)
          .sendMessage(message)
          .then((res) => {
            this.$store.commit("pushCurrentMessageList", message);
          })
          .catch((error) => {
          });
      }
    },
```

利用自定义消息可以实现腾讯云文档中不包含的消息类型根据业务需求发送消息，比如实现智能客服自定义消息类型，远程消息类型等等。

#### **普通消息：**

腾讯云接口文档有提供消息api,可以根据api进行消息的发送。

| API                                                          | 描述                                |
| :----------------------------------------------------------- | :---------------------------------- |
| [createTextMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#createTextMessage) | 创建文本消息。                      |
| [createTextAtMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#createTextAtMessage) | 创建可以附带 @ 提醒功能的文本消息。 |
| [createImageMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#createImageMessage) | 创建图片消息。                      |
| [createAudioMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#createAudioMessage) | 创建音频消息。                      |
| [createVideoMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#createVideoMessage) | 创建视频消息。                      |
| [createCustomMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#createCustomMessage) | 创建自定义消息。                    |
| [createFaceMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#createFaceMessage) | 创建表情消息。                      |
| [createFileMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#createFileMessage) | 创建文件消息。                      |
| [createMergerMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#createMergerMessage) | 创建合并消息。                      |
| [downloadMergerMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#downloadMergerMessage) | 下载合并消息。                      |
| [sendMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#sendMessage) | 发送消息。                          |
| [revokeMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#revokeMessage) | 撤回消息。                          |
| [resendMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#resendMessage) | 重发消息。                          |
| [deleteMessage](https://web.sdk.qcloud.com/im/doc/zh-cn/SDK.html#deleteMessage) | 删除消息。                          |

### 会话

#### 获取消息列表。

分页拉取指定会话的消息列表的接口，当用户进入会话首次渲染消息列表或者用户“下拉查看更多消息”时，需调用该接口。

我这里做了点击'获取更多'获取历史消息，当前页面一次只加载20条消息，提升页面加载速度。

```js
 //方法放在了vuex里，页面需要获取更多历史消息，调用vuex里的异步方法获取数据
    /**
     * 获取消息列表
     * 调用时机：打开某一会话时或下拉获取历史消息时
     * @param {Object} context
     * @param {String} conversationID
     */
    getMessageList(context, conversationID) {
      if (context.state.isCompleted) {
        return
      }

      const {
        nextReqMessageID,
        currentMessageList
      } = context.state
      tim(store.state.basic.imInfo.sdkAppID).getMessageList({
        conversationID,
        nextReqMessageID,
        count: 15
      }).then(imReponse => {
        console.log("getMessageList", imReponse);
        // 更新messageID，续拉时要用到
        context.state.nextReqMessageID = imReponse.data.nextReqMessageID
        context.state.isCompleted = imReponse.data.isCompleted
        // 更新当前消息列表，从头部插入
        context.state.currentMessageList = [...imReponse.data.messageList, ...currentMessageList]
        context.state.pageList = [...imReponse.data.messageList]
      })
    },
```

#### 获取会话列表。

获取会话列表的接口，该接口会尝试同步最新的100条会话，在同步完成后返回 SDK 内部维护的会话列表。 调用时机：需要刷新会话列表时

注意：会话保存时长跟会话最后一条消息保存时间一致，消息默认保存7天，即会话默认保存7天。

```js
// 拉取会话列表
let promise = tim.getConversationList();
promise.then(function(imResponse) {
  const conversationList = imResponse.data.conversationList; // 会话列表，用该列表覆盖原有的会话列表
}).catch(function(imError) {
  console.warn('getConversationList error:', imError); // 获取会话列表失败的相关信息
});
```

#### 获取会话资料。

获取会话资料的接口，当点击会话列表中的某个会话时，调用该接口获取会话的详细信息。

```js
let promise = tim.getConversationProfile(conversationID);promise.then(function(imResponse) {  // 获取成功  console.log(imResponse.data.conversation); // 会话资料}).catch(function(imError) {  console.warn('getConversationProfile error:', imError); // 获取会话资料失败的相关信息});
```

### 界面消息类型

消息框中可以实现表情、图片、视频、文档消息发送。

![在这里插入图片描述](https://img-blog.csdnimg.cn/46b2b15f2ecf4ddaa7475cea0dac34df.png) 

![在这里插入图片描述](https://img-blog.csdnimg.cn/641e00c18d43411fa315ed93729c4038.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

在`message-item`中定义每一条消息的发送样式，包括头像是否显示、是否是系统消息等等。通过组件的方式，给每一种消息类型定义不同的样式。

![在这里插入图片描述](https://img-blog.csdnimg.cn/9504060c07a44a51b91f97ac06f6a886.png) 

![在这里插入图片描述](https://img-blog.csdnimg.cn/37c7815a691045f2956e6ded65acb666.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/fe9d4a9ec1e8490bb87e052aded2b439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

```js
  avatar() {
      if (this.currentConversation.type === "C2C") {
        if (this.isMine) {
          return this.sessionObj.guestAvatar;
        } else {
          return this.sessionObj.serviceAvatar;
        }
      } else if (this.currentConversation.type === "GROUP") {
        return this.isMine
          ? this.currentUserProfile.avatar
          : this.message.avatar;
      } else {
        return "";
      }
    },
    currentConversationType() {
      return this.currentConversation.type;
    },//头像
    
    isMine() {
      // console.log(this.message.from,this.imInfo.userID,this.message.from == this.imInfo.userID)
      return this.message.from == this.sessionObj.guestImAccount;
    },//是否是自己发的消息

    messagePosition() {
      if (
        ["TIMGroupTipElem", "TIMGroupSystemNoticeElem"].includes(
          this.message.type
        )
      ) {
        return "position-center";
      }

      if (this.message.type == "TIMCustomElem") {
        if (
          this.message.payload.data.subMsgType == "prompts" ||
          this.message.payload.data.subMsgType == "transfer" ||
          this.message.payload.data.subMsgType == "reception" ||
          this.message.payload.data.subMsgType == "stopsession" ||
        ) {
          return "position-center";
        }
      }
      if (this.message.isRevoked) {
        // 撤回消息
        return "position-center";
      }
      if (this.isMine) {
        return "position-right";
      } else {
        return "position-left";
      }
    },    //消息位置
```

### 智能客服会话

在`message-item`中定义定义自定义消息。

```js
            <!-- 智能客服自定义热度问题 -->
            <custom-heatquestion
              v-else-if="message.subMsgType === 'heatquestion'"
              :isMine="isMine"
              :payload="message.msgContent"
              :message="message"
            />
            <!-- 智能客服自定义相似问题 -->
            <custom-similarquestion
              v-else-if="message.subMsgType === 'similarquestion'"
              :isMine="isMine"
              :payload="message.msgContent"
              :message="message"
            />
            <!-- 智能客服自定义查看指定问题 -->
            <custom-specifiedquestion
              v-else-if="message.subMsgType === 'specifiedquestion'"
              :isMine="isMine"
              :payload="message.msgContent"
              :message="message"
            />
```

界面上判断是否开启智能客服，通过维护知识库内容实现点击问题进行回答：

![在这里插入图片描述](https://img-blog.csdnimg.cn/75b09b5a991a43bea705315d8afedf29.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/021c2af7ae084281a07423fa7a549dfd.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

点击**转人工**按钮可以接通人工客服。

### 远程控制

远程控制功能主要以客户端插件的方式实现客服控制访客桌面，进行远程控制。

这里采用了向日葵的远程插件，使用者需要先申请创建`APPID` 和` APP KEY`。

大致流程图：

远程控制主要是客服端向访客发起，访客可以选择接受或者拒绝，并且和实时系统消息进行关联，分为远程开始、进行中、结束以及访客拒绝。

![在这里插入图片描述](https://img-blog.csdnimg.cn/c5cd062ace4843f6a5593dc6a6bf29ff.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/677770e93c0d46148455176133d0c6f5.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/fc0d9a80cb0a440cad593258efbf39f3.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/7846ceb741364702a89a5b6e35f9e4e1.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

远程进行中会进行轮巡判断当前远程是否进行，否则关闭远程。

这里主要也是以自定义消息进行远程会话，然后调用客户端方法打开插件。源码中使用了`external.calld`的方法都是调用了后端的客户端方法，需要实现的小伙伴需要和后端进行沟通。


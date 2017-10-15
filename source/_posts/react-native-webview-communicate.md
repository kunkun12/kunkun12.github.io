title: react-native与webview的双向通信
date: 2017-03-14 00:21:25
tags:
---
react native 在官网的文档中，并没有详细介绍<http://facebook.github.io/react-native/docs/webview.html#onmessage>如何实现RN与WebView中JavaScript的通信。 关于 onMessage的描述如下，

        onMessage?: function 
        A function that is invoked when the webview calls window.postMessage. Setting this property will inject a postMessage global into your webview, but will still call pre-existing values of postMessage.
        window.postMessage accepts one argument, data, which will be available on the event object, event.nativeEvent.data. data must be a string.

从上只能看出，RN可以接受来自于webview的消息，然而并无法得知如何从RN往Webview内部发送消息，读源码的时候，发现了 RN的webview组件中除了`onMessage` 还有个`posMessage`方法，由此可以推出能搞定RN与Webview内H5的双向通信<https://github.com/facebook/react-native/blob/0150bc76eb8d2baa84ba040aead8131228c1185e/Libraries/Components/WebView/WebView.ios.js#L506>
- 在RN中 `onMessage`负责接收Weview的消息 `postMessage` 负责发送消息给Webview
- 通过监听`message`事件来接收RN的消息，通过`window.postMessage`在Webview中向RN中发送消息 

于是写代码验证下。在html中放置两个输入框A、B,点击按钮把A、B的值发送给RN端，返会两个数的和并展示

#### H5的代码

```
    <div style="padding-top:50px;">
        <input id='A' /> 
        <input id='B' />
    </div>
  <Button id='cacl'>计算</Button> <span>AB相加为</span><span id='sum'></span>
```
```
      var bt= document.getElementById('cacl');
      bt.addEventListener('click',function(){
        var valA = document.getElementById('A').value;
        var valB = document.getElementById('B').value;
        window.postMessage(JSON.stringify({
              type:'add',
              data:{
                A:valA,
                B:valB
              }
            }))
      });
     document.addEventListener('message',function(e){
        document.getElementById('sum').innerText = JSON.parse(e.data).result;
     })
```

#### RN 中的代码 
```
 export default class RNWebbViewMessage extends Component {
  constructor(props){
    super(props);
      this.onMessage = this.onMessage.bind(this);
  }
   onMessage(e){
      var event =e.nativeEvent;
      var data=JSON.parse(event.data);
      if(data.type ==='add'){
        let  args= data.data;
        let a = Number(args.A);
        let b = Number(args.B);
        this.refs.webviewRef.postMessage(JSON.stringify({
            result:a+b
         }))
      }
    }
  render() {
    return (
      <View style={{flex:1}}>
          <WebView ref={'webviewRef'} automaticallyAdjustContentInsets={false} onMessage={this.onMessage} javaScriptEnabled={true} source={{uri:'http://localhost/work/study/rnwebviewmessage/index.html?ss'}} /> 
      </View>
    );
  }
}
```

经过验证结论是成立的。
- webview网页可以使用`window.postMessage` 发送消息， `document.addEventListener('message',callback)`来监听消息
- RN 中onMessage 通过 参数中nativeEvent.data获得webview传递过来的字符串。然后把结果通过webview实例的postMessage方法发送到H5中

以上只是通信api的介绍，如果生产环境中使用的话，还可以对调用形式进一步封装，比如H5中方法调用RN中的分享，并且要求知道分享结果是否成功，可以使用 `webviewBridgecallRN('share',{title:'微信分享'，content:"分享内容"},callback)`这种类似的方式，同时RN中可根据事件的名字进行分模块来实现。

##### 备注
- 查阅了react-native的Webview源码变更记录  `onMessage/postMessage`是从 0.37 版才支持的。android与ios的支持版本同步。
- 之前rn webview 通信可以使用 [https://github.com/alinz/react-native-webview-bridge](react-native-webview-bridge)
 

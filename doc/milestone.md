## wxagent项目简单后续计划

2015-09-29

### wxagent现状

wxagent项目，从开始开发至今，wxagent已经能够基本实现微信消息与tox消息的互通。

比较好的实现了多媒体类型的消息，包括传文件，传图片，传动画表情，传语音消息。

并且，wxagent实现的程序架构模式，让其更灵活，可以运行在本机，VPS，或者云端docker容器中，

正如readme.md中所说，常用的IM可以一直在线了，不用繁(bi)索(ge)的扫码，

在你想偶尔注销一下Linux桌面时，不用纠结下次再次登陆微信桌面端了。

并且，在一下节中你还会看到，不但能够与tox实现互通，还能够与其他一些开放的IM协议互通，

像XMPP协议，IRC协议，或者目前新的telegram，wathsapp等。


### wxagent不足，改进之处

##### 安装配置

目前，wxagent本身不需要配置就能够运行，在于互通端需要有另一个IM的账号，

应该让这个互通IM账号很容易的配置并使用，让用户用最喜欢使用的IM客户端实现互通，

wxagent则在后台负责双向中继并转发IM消息，达到无缝完美互通的目标。

甚至还有一种可能，实现一个公用的服务，为不同用户提供微信中继功能，把可配置化进一步改进成服务化。

##### 消息重新格式化

微信的消息格式是它所支持的格式，除了基本的文本消息之外，还包括了大量丰富的消息类型，

像文章订阅，收到的消息内容为一段XML格式数据，表示消息不同的属性。

这时候要通微信消息提取组件，提取相应的属性信息与消息内容，再通过中继IM协议消息打包组件，

转换为最终接收消息的IM协议类型消息，实现比较理想的消息兼容展示功能。


##### 消息的暂存与历史接收

假如本程序做为daemon长时间运行，在该程序的客户端没有启动的时候，当前的机制是收到的消息会丢失，

客户端上线后，也不会再收到上线前的任何消息通过。

可以考虑这种消息的暂存功能，客户上线时全部推送给客户程序。

那么客户将在上线的时刻收到大量的信息，体验也不太好。

或者还有一个可能，用户上线时提示历史消息的个数，让用户选择推送哪些好友的暂存的消息记录。

这里要加新的交互功能，即选择推送暂存的好友消息功能，具体细节还需要再考虑。

另外，在实现上，考虑在relay层暂存消息更好，具体的暂存功能细节还需要再考虑。


##### 其他中继IM协议支持

目前已经比较好的支持tox协议了，但是由于tox的P2P特性，在不够成熟时，发现在有些某络情况下还不太稳定。

因此就考虑了使用其他类型的中继IM协议的可能性，目前已经对XMPP协议做了原型开发测试，效果还比较好。

其他的IRC协议，已经了解过一些，后续将有时间再跟进，应该是可行的。

在使用了多协议的中继IM协议后，从代码上，最好应该把公用的中继逻辑代码抽像出来，

再把不同的中继IM协议按照中继逻辑接口的需求，封装成不同的中继协议类型，方便中继逻辑层可配置的选择不同的中继IM协议。

目前，对中继IM协议有一定要求，需要支持群组聊天功能才可以加入到中继IM框架中。

##### tecent的相关产品的考虑

tecent的QQ，使用那是更广泛了，再加上微信，算是两个IM，成为了国内主要的IM载体。

不过，从tecent历来的在互联网圈的表现，对其IM采取保守与封闭的模式，并对其IM产品协议的第三方逆向工程工作有不友好的处理。

这种公司规模大了，从某个角度来说，也可能对个人用户不是件好事情呢。

虽然这样，本着占了便宜还卖乖的原（wu）则（nai），还需要继续使用QQ和微信的。

那就用另一种方式来使用吧，由于目前能够使用的tecent两个IM协议的实现限制，这种使用方式可能还不够完善。

最终，除了wxagent之外，准备再推出qqagent，架构模式与wxagent类似，但承载QQ协议的中继功能。

还有可能，最终这两个agent，合而为一，成为针对tecent公司IM产品的综合中继与开放方案。


##### 更好的客户端体验

改进不同中继IM协议的客户端，或者可以使用插件体系，比较好的显示格式化的消息或者丰富的图片表现形式。

呃，好像有点太多了。


# 🔹🔹超级简单的静态住宅 IP 配置教程🔹🔹

教程更新日期：2025/12/17

网络上很多教程都是基于 relay 去讲解如何配置静态住宅 IP，但是 Clash Verge 升级后 relay 类型的 proxy-group 已经被废弃，教程也随之失效。

这篇教程会教你如何通过 dialer-proxy 建立网络连接，别被这些看不懂的英文名字吓到，教程简单快捷，有手就行。

## 1/ 开始前需要准备

- Clash Verge（最新版）https://github.com/clash-verge-rev/clash-verge-rev
- 机场订阅（贴一个机场测速的网站，如果还没有订阅可以参考）https://www.duyaoss.com
- 静态住宅 IP（目前自用）https://cliproxy.com/zh

## 2/ 添加机场订阅

- 打开 Clash Verge 左侧「订阅」页面添加机场订阅。
- 绝大多数的机场都有一键导入功能，如果没有就需要复制订阅文件链接粘贴到 Clash Verge 自行导入。
<img width="950" height="308" alt="截屏2025-12-17 16 12 11" src="https://github.com/user-attachments/assets/bbadc666-af92-4ef9-a8f8-28879a1edd36" />

## 3/ 提取机场文件「自动选择」代理组

- 打开 Clash Verge 左侧「订阅」页面，点击你刚才打入的机场订阅文件，弹出的快捷菜单中选择「编辑文件」。
- 找到 proxy-groups 这一行，复制「自动选择」一整行粘贴到新建的记事本文档里，不必保存，稍后就要用到，如果机场文件没有「自动选择」这个名称，找 type 类型为 url-test 的代理组复制粘贴。url - test 就是通过 url 自动测速选择节点，这样后边我们配置好静态住宅 ip 后就不用自己手动切换前置的机场节点了。 
<img width="384" height="80" alt="截屏2025-12-17 16 24 13" src="https://github.com/user-attachments/assets/973e3157-c075-47db-9dbd-57219370c9e6" />

- 复制出来的代码应该是这个样子的

    '- { name: 自动选择, type: url-test, proxies: [香港-01, 香港-02, 这里会有很多节点], url: 'http://www.abc.com', interval: 86400 } '

## 4/ 修改 Merge 文件

- 打开 Clash Verge 左侧「订阅」页面，打开「全局扩展覆写配置」，复制下面代码粘贴替换掉原有代码。
- 根据代码里的文字提示替换节点信息，保存关闭后点击「订阅」页面右上角的刷新按钮。

```# Profile Enhancement Merge Template for Clash Verge

profile:
  store-selected: true

proxy-groups:
  ########################################
  # 「生生不息」可以替换成你想要创建的代理组名称，我们会在这个代理组配置你的静态住宅 ip
  # 「自动选择」这一行替换成你刚才在机场文件复制粘贴的
  ########################################
  - { name: 生生不息, type: select, use: ["provider1"] }
  - { name: 自动选择, type: url-test, proxies: [香港-01, 香港-02, 这里会有很多节点], url: 'http://www.abc.com', interval: 86400 } 
proxy-providers:
  provider1:
    type: inline
    override:
      dialer-proxy: 自动选择
    payload:
      - { name: Proxy-US, dialer-proxy: dialer, type: socks5, server: 1.1.1.1, port: 443, username: imqixia, password: loveme }
  ########################################
  # payload 下面定义的是你的静态住宅 ip
  # Proxy-US 是你的静态 ip 名称可以自行修改，server 是 ip 地址，port 端口，username 用户名，password 密码，这些都可以在静态住宅 ip 购买网站的后台找到，自行修改
  # rules 可以直接复制粘贴使用，记得把生生不息替换成你创建的代理组名称，需要修改规则可以直接找 ai 帮忙 diy
  ########################################
rules:
  - MATCH,生生不息
```

## 5/ 测试节点是否通畅

- 打开左侧「代理」页面，会显示你新创建的代理组以及静态住宅 ip 节点，点击选择，check 测速，看一下延迟。
- 测通之后根据自己需求选择规则还是全局模式。
- 左侧「首页」页面，在「网络设置」里面打开「系统代理」按钮，代理就正式生效了。「虚拟网卡模式」根据自己需求选择使用或者不用。
<img width="968" height="406" alt="截屏2025-12-17 16 51 09" src="https://github.com/user-attachments/assets/39523c01-5189-4a8e-bc75-07abe7291bd9" />

## 6/ 检查静态住宅 ip 节点

- 打开检测 ip 网站，检查自己的 ip 是否为刚才配置的静态住宅 ip，以及这个 ip 的纯净度。这是使用的是 [ping0.cc](ping0.cc)

<img width="998" height="336" alt="截屏2025-12-17 16 57 45" src="https://github.com/user-attachments/assets/de4e016c-6329-4882-b885-2c5009f27f77" />

## 7/ 参考文档

- 如果你对以上 Merge 代码处有疑问，可以去看下说明文档，这里有对代码块的详细讲解，你也可以对代码做升级和自定义。
- https://wiki.metacubex.one/config/proxies/dialer-proxy/
- 科学上网是一门科学，你都不学，谈什么科学上网呢？非常靠谱的科学上网系列视频，有兴趣可以去看下。
- https://www.youtube.com/playlist?list=PLqybz7NWybwUgR-S6m78tfd-lV4sBvGFG

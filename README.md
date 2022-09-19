# 免责声明

1. 本仓库发布的 `sheep-analysis` (下文均用本项目代替) 项目中涉及的任何脚本，仅用于测试和学习研究，禁止用于商业用途，不能保证其合法性，准确性，完整性和有效性，请根据情况自行判断
2. 本项目内所有资源文件，禁止任何公众号、自媒体进行任何形式的转载、发布，禁止直接改项目名二次发布。
3. 作者对任何脚本问题概不负责，包括但不限于由任何脚本错误导致的任何损失或损害.
4. 请勿将本项目的任何内容用于商业或非法目的，否则后果自负。
5. 如果任何单位或个人认为该项目的脚本可能涉嫌侵犯其权利，则应及时通知并提供身份证明，所有权证明，我们将在收到认证文件后删除相关脚本。
6. 以任何方式查看此项目的人或直接或间接使用本项目的任何脚本的使用者都应仔细阅读此声明。作者保留随时更改或补充此免责声明的权利。一旦使用并复制了任何相关脚本或本项目，则视为您已接受此免责声明
7. 您必须在下载后的24个小时之内，从您的电脑或手机中彻底删除上述内容。
8. 本项目遵循GPL-3.0 License协议，如果本特别声明与GPL-3.0 License协议有冲突之处，以本特别声明为准。
9. 任何擅自改变计算机信息网络数据属于违法行为，本项目不提供成品可运行程序，仅做学习研究使用。
`您使用或者复制了本仓库且本人制作的任何代码或项目，则视为已接受此声明，请仔细阅读。`


# sheep-analysis

> 中间人攻击（Man-in-the-Middle Attack, MITM）是一种由来已久的网络入侵手段，并且当今仍然有着广泛的发展空间，如SMB会话劫持、DNS欺骗等攻击都是典型的MITM攻击。简而言之，所谓的MITM攻击就是通过拦截正常的网络通信数据，并进行数据篡改和嗅探，而通信的双方却毫不知情。

这种攻击方式可以篡改请求和响应数据，这就是为什么不要轻易连接公共场所的免费无密码wifi的原因

# 模拟操作

> 本文中使用的是MAC + IOS环境

> 再次声明：仅供学习研究，禁止非法使用

此方式是将第二关内容变成第一关内容

1. 下载 mitmproxy
```
brew install mitmproxy
```
2、配置响应拦截

抓包将第一关的json数据保存到/Users/你的用户名/Downloads 目录下更名为 sheep.json

第二关地图URL通过抓包获得

将代码放到下载目录下 /Users/你的用户名/Downloads 更名为 sheep.py
```
from mitmproxy import ctx
import json
 
class ModifyResponse:
    def response(self,flow):
        #拦截指定的url
        if flow.request.url.startswith("第二关地图URL"):
            #返回数据json，绝对路径
            with open('/Users/你的用户名/Downloads/sheep.json','rb') as f:
                res = json.load(f)
            #设置返回数据
            flow.response.set_text(json.dumps(res))
            #log中打印
            ctx.log.info('modify words-template response')
 
addons = [
    ModifyResponse()
]

```

3、启动代理

```
mitmweb -s /Users/你的用户名/Downloads/sheep.py
```

4、连接代理
可百度查询ipone如何连接代理

代理端口：8080

5、下载代理证书（在成功连接代理后）

手机浏览器访问 mitm.it 

下载对应系统证书

在ios系统【通用】-【VPN与设备设置】找到mitmproxy证书安装

在ios系统【通用】 - 【关于本机】- 【证书信任设置】将mitmproxy证书设置为信任

6、打开小程序

自行操作即可

# 结束语
`此文目的是为了让大家提升安全意识，防止因为安全意识的缺失导致自身信息安全的侵害`

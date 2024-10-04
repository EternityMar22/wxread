## 序

开发这个脚本主要是为了挑战赛刷时长以及天数，由于本人偶尔看书有时来不及签到导致入场费打了水漂。网上找了一些相关代码，发现高赞的自动阅读需要挂电脑或者用ADB模拟，一点也不优雅。于是抓了一下官网接口，分析了官网的源码JS逆向了一部分代码，编写了Python挂机脚本兼有自动更新Cookie与推送到微信的功能。只需要一次部署，长时间挂机。

## 脚本介绍

针对微信读书阅读挑战赛编写的脚本：

1. 能进行刷阅读时长，且时长默认计入排行榜、挑战赛等。（指定200分钟）
2. 可部署服务器每天定时运行脚本并推送到微信。
3. 一次抓包，长时间使用。对于Cookie更新问题给出了自动获取Cookie更新值。
4. 比较市面上的ADB调试器、自动阅读器，本脚本实现了轻量化编写，部署服务器即可运行，无需更多条件。
5. 脚本JS逆向分析各接口请求，分析各字段的拼接方式，并对字段进行加密、计算处理使得服务器能够成功响应（`{'succ': 1, 'synckey': 2060028311}`，表示数据字段正常）。

## 操作步骤

1、脚本逻辑还是比较简单的，运行`main.py`即可，依赖自行安装。大部分代码不需要改动，在微信阅读官网 [微信读书 (qq.com)](https://weread.qq.com/) 搜索【三体】点开阅读点击下一页进行抓包，抓到`read`接口 `https://weread.qq.com/web/book/read`，如果返回格式正常（如：

```
json复制代码{
  "succ": 1,
  "synckey": 564589834
}
```

右键复制为Bash格式，然后在 [Convert curl commands to Python (curlconverter.com)](https://curlconverter.com/python/) 转化为Python脚本，只复制前面的Header与Cookie字段替换到`main.py`即可。对于`renew`（`https://weread.qq.com/web/login/renewal`）自动更新Cookie的脚本，可以沿用`main.py`中的大部分字段，或者自己抓取，需要一段时间（大于一小时）。

2、服务器运行，在你的服务器上有Python运行环境即可，使用`cron`定义自动运行指定即可。（如：

```
bash


复制代码
0 2 * * * /www/server/pyporject_evn/wxread_venv/bin/python3 /root/wxread/main.py >> /root/wxread/logs/$(date +\%y-\%m.\%d)_sout.log 2>&1
```

可供参考）。

3、Pushplus推送，更改你的Token即可。

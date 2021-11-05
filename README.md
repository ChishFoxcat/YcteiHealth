<div align="center"> 
<h1 align="center">
🌈YCGY健康打卡
</h1>

![](https://img.shields.io/github/stars/ChishFoxcat/YcteiHealth?style=social "Star数量")
![](https://img.shields.io/github/forks/ChishFoxcat/YcteiHealth?style=social "Fork数量")
[![LICENSE](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square&logo=github&label=LICENSE)](https://github.com/ChishFoxcat/YcteiHealth/blob/master/LICENSE "License")
[![Releases](https://img.shields.io/github/v/release/ChishFoxcat/YcteiHealth?style=flat-square)](https://github.com/ChishFoxcat/YcteiHealth/releases "Release")

</div>

<h1 align="center">

[![Source](https://img.shields.io/badge/Source%20by-Github%20Pages%20Go%E2%86%92-gray.svg?colorA=424242&colorB=4CAF50&style=for-the-badge)](https://github.com/ReaJason/17wanxiaoCheckin)

</h1>

## ⚠️声明

* 仅用于日常健康打卡,禁止使用于其他用途
* 非盈利项目,严禁倒卖及获取利益,任何责任与二次编辑和原作者无关

## 🔰项目功能

* [x] 完美校园模拟登录获取 token
* [x] 自动获取上次提交的打卡数据，也可通过配置文件修改
* [x] 支持健康打卡和校内打卡
* [x] 单人自定义推送，可统一推送
* [x] 支持粮票签到收集，自动完成查看课表和校园头条任务
* [x] 支持邮箱、Qmsg、Server 酱、PipeHub 推送打卡消息

## 🎨配置文件

### 💃用户配置

- 打卡用户配置文件位于：`conf/user.json`
- 整个 json 文件使用一个 `[]` 列表用来存储打卡用户数据，每一个用户占据了一个 `{}`键值对，初次修改务必填写的数据为：`phone`、`password`、`device_id`、健康打卡的开关，校内打卡开关（有则开），推送设置 `push`。
- 关于 `post_json`，如若打卡推送数据中无错误，则不用管，若有 null，或其他获取不到的情况，则酌情修改即可，和推送是一一对应的。

设备id认证软件：[点我下载]()
（根据截图判断自己属于哪一类[【1】](https://cdn.jsdelivr.net/gh/ReaJason/17wanxiaoCheckin/Pictures/one.png)、[【2】](https://cdn.jsdelivr.net/gh/ReaJason/17wanxiaoCheckin/Pictures/two.png)）

### 🤝统一推送配置

- 统一推送配置文件位于：`conf/push.json`
- 若多用户打卡使用统一推送而不是个别单独推送则在此文件下进行推送的配置




- 第一类健康打卡成功结果：`{'msg': '成功', 'code': '10000', 'data': 1}`，显示打卡频繁也算

- 第二类健康打卡成功结果：`{'code': 0, 'msg': '成功'}`

- 校内打卡成功结果：`{'msg': '成功', 'code': '10000', 'data': 1}`

- 如果你们学校会记录打卡成功与否可直接在 **支付宝小程序** 查看是否记录上去（手机 app 登录的话之前获取的 device_id 就失效了）

- 最后检查推送数据，如果表格中有 None，请根据第二行的信息，搭配第一行推送信息的格式，修改配置文件

  - 打开第一行，找到 updatainfo 这个东西，下面的有 null 的对应就是表格中的 None，记住它的 propertyname

  - 打开第二行，找到对应 perpertyname 的部分，根据 checkValue 的 text 选择你需要的选项，温度自己填个值就可

  - 打开配置文件，找到 post_json 下的 updatainfo，在里面加入你需要修改的值，格式和第一行里面的打卡数据一样

  - ```
    "updatainfo":[
    	{
        	"propertyname": "temperature",  // 这个为第一行中找到值为 null 的那一项
            "value": "35.7"  // 这个值为你想改的值，第二行中获取，如果是温度，自己填自己想的即可
        },
        {
        	"propertyname": "wengdu",
        	"value": "36.4"
        }
    ]
    ```

     

- 由于前面使用软件获取了 device_id，所以请使用 **支付宝小程序** 查看打卡结果是否记录上去，以免手机登录 device_id 失效


## Crontab 定时任务
1. 首先在命令行执行 `crontab -e` 打开 crontab 的配置文件
2. 然后按 `i` 进行编辑
3. 在 crontab 的配置文件中的新一行输入 `0 5 * * * /home/YcteiHealth/health.sh > /dev/null 2>&1` 这一句

> - `0 5 * * *` 是指 0 分钟 5 时 每天 每月 每日
> - `/home/YcteiHealth/health.sh` 是定时执行所需脚本
> - `> /dev/null 2>&1` 是禁止系统在执行完命令后在 /tmp 文件夹下创建日志文件记录执行状况

## 脚本篇 (health.sh)
1. 前往签到脚本所在目录 `cd /home/YcteiHealth`
2. 在脚本目录下创建一个文件名为 `health.sh` 的 .sh 脚本
3. 在脚本开头写个 bash 文件头，指定脚本类型 `!#/bin/bash`
4. 在脚本 `health.sh` 的新的一行中写 `screen -dmS YcteiHealth /bin/bash -c "cd /home/YcteiHealth;python index.py;exec /bin/bash"`
5. 保存！

> - `screen -dmS YcteiHealth /bin/bash` 创建一个名为 YcteiHealth 的 screen 守护进程，类型为 bash 命令行
> - `-c` 执行命令
> - `"cd /home/YcteiHealth;python index.py;exec /bin/bash"` /tp 到打卡脚本的目录下，用 python 运行名为 index.py 的 python 脚本，最后再执行运行 /bin/bash 来保证脚本运行完能恢复会话查看执行结果。（命令间用 ; 隔开）

## 宝塔定时任务出现的问题
- **查看文章失败的错误**
> 宝塔的定时任务其实也是使用系统的 `crontab` 来执行，但直接在脚本内容中 cd 到目录再执行脚本可能会出现莫名其妙的读取不到配置文件的错误（与在路由器上用 screen cd 到 frpc 目录但是找不到目录的问题类似）

## 🙋‍脚本有问题
* 有问题可提 [issue](https://github.com/ChishFoxcat/YcteiHealth/issues)

## 📜FQA

- 若第一类健康打卡或校内大卡打卡推送显示，需要修改对应位置下的 areaStr，修改格式为：`"areaStr": "{\"address\":\"天心区青园路251号中南林业科技大学\",\"text\":\"湖南省-长沙市-天心区\",\"code\":\"\"}"` ，`address`：对应手机打卡界面的下面一行，`text`：对应手机打卡界面的上面一行，根据自己的来，上面填什么就是什么，若是校内打卡的地址获取不到，可查看健康打卡的打卡数据推送里面的 areaStr 复制即可。
- 若打卡结果为 `{'msg': '参数不合法', 'code': '10002', 'data': ;'deptid can not be null'}`，初步认为你们学校打卡数据无法自动获取，每次需要自己填写数据，解决办法为手机登录打卡抓签到包，然后在配置文件的 `post_json` 中填下你的打卡数据。


### **[详情点击我跳转到源项目查看MarkDown](https://github.com/ReaJason/17wanxiaoCheckin)**

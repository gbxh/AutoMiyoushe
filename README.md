# 米游社辅助签到

### 使用Json格式的用户请注意，在版本更新后配置文件格式从json换成yaml了，需要安装`PyYAML`模块脚本才能正常工作
- 本地运行使用 `pip install PyYAML` 上传云函数运行则使用 `pip install PyYAML -t .`

基于Python3的米游社辅助签到项目

禁止大范围宣传本项目，谢谢配合

本项目米游币部分参考[XiaoMiku01/miyoubiAuto](https://github.com/XiaoMiku01/miyoubiAuto)进行编写

* 此项目的用途

  这是一个米游社的辅助签到项目，包含了米游币、崩坏2、崩坏3、原神、未定事件簿
  已经支持米哈游国内正在运营的全部游戏的米游社签到(2022-7-19)

## 如何使用程序

* **部署方法**

  1. 使用[Git](https://git-scm.com/)或[点击此处](https://github.com/Womsxd/AutoMihoyoBBS/archive/refs/heads/master.zip)下载本项目

  2. 下载[Python3](https://www.python.org/downloads/)

  3. 解压本项目压缩包,在解压目录中**Shift+右键** 打开你的命令提示符cmd或powershell

  4. [requirements.txt](https://raw.githubusercontent.com/Womsxd/AutoMihoyoBBS/master/requirements.txt) 是所需第三方模块，执行 `pip install -r requirements.txt` 安装模块

  5. 打开目录中的**config文件夹**复制`config.yaml.example`并改名为`config.yaml`，脚本的多用户功能靠读取不同的配置文件实现，你可以创建无数个`自定义名字.yaml`，脚本会扫描**config**目录下`yaml`为拓展名的文件，并按照名称顺序依次执行。

  6. 请使用vscode/notepad++等文本编辑器打开上一步复制好的配置文件

  7. **使用[获取Cookie](#获取米游社Cookie)里面的方法来获取米游社Cookie**

  8. 将复制的Cookie粘贴到`config.yaml`的`cookie:" "`中(在`account`里面)

        例子

        > ```yaml
        > cookie: 你复制的cookie
        > ```

  9. 检查`config.yaml`的`enable:`的值为true

  10. 在命令提示符(cmd)/powershell，输入`python main.py`来进行执行
  
  11. 多用户的请使用`python main_multi.py`，多用户在需要自动执行的情况下请使用`python main_multi.py autorun`

## 获取米游社Cookie

1. 打开你的浏览器,进入**无痕/隐身模式**

2. 由于米哈游修改了bbs可以获取的Cookie，导致一次获取的Cookie缺失，所以需要增加步骤

3. 打开`http://bbs.mihoyo.com/ys/`并进行登入操作

4. 在上一步登入完成后新建标签页，打开`http://user.mihoyo.com/`并进行登入操作 (如果你不需要自动获取米游币可以忽略这个步骤，并把`mihoyobbs`的`enable`改为`false`即可)

5. 按下键盘上的`F12`或右键检查,打开开发者工具,点击Console

6. 输入

   ```javascript
   var cookie=document.cookie;var ask=confirm('Cookie:'+cookie+'\n\nDo you want to copy the cookie to the clipboard?');if(ask==true){copy(cookie);msg=cookie}else{msg='Cancel'}
   ```

   回车执行，并在确认无误后点击确定。

7. **此时Cookie已经复制到你的粘贴板上了**

## 获取设备UA

1. 使用常用的移动端设备访问 `https://www.ip138.com/useragent/`

2. 复制网页内容中的 `客户端获取的UserAgent`

3. 替换配置文件中 `useragent` 的原始内容

## 使用Docker运行

Docker的运行脚本基于Linux平台编写，暂未在Win平台测试。

将本项目Clone至本地后，请先按照上述步骤添加或修改配置文件。随后执行

```text
docker-compose up -d
```

启动docker容器。  
&nbsp;  
容器运行成功后可用

```text
docker-compose logs -f
```

命令来查看程序输出。  

若需要添加配置文件或修改配置文件，可直接在主机config文件夹中修改，修改的内容将实时同步在容器中。

每次运行Docker容器后，容器内将自动按照参数执行签到活动，签到完成后容器将默认在每天上午9:30运行一次，如果想自行修改时间可自行编辑`docker-compose.yml`文件中的`CRON_SIGNIN`，将其修改成想运行的时间。

若想要更新容器镜像，可以参考以下命令

```text
docker-compose stop  
docker-compose pull && docker-compose up -d
```

## 使用python运行(screen)

1. 将本项目Clone至本地后，安装好依赖直接运行`python3 server.py`

2. 在后台运行时请安装screen  

3. 使用`screen -S automhy`进入后台线程  

4. Ctrl+A组合键再按下d键回到主线程  

5. `screen -r automhy`回到线程  

6. 如果不能回到线程请先`screen -d automhy`挂起线程  

### 命令窗口如下  

   >stop: 关闭程序  
   >mulit: 测试多用户签到  
   >single: 测试单用户签到  
   >reload: 重载配置文件  
   >mod x: mod 1为单用户模式 mod 2为多用户模式  
   >add 'yourcookie': 直接 add cookie 添加Cookie，根据提示输入用户存档名称  
   >time x: 设置任务巡查时间,默认720分钟(12小时)  
   >set user enable true(设置user.json 的enable属性为true)  
   >show true/false: 开启/关闭20秒的倒计时提示

## 使用云函数运行

腾讯云函数服务免费额度近期有变化，为了**避免产生费用**，建议切换到阿里云 函数计算 FC

* 腾讯云

1. 下载本项目

2. 在脚本目录执行`pip3 install -r requirements_qcloud.txt -t .`

3. 在本地完整运行一次。

4. 打开并登录[云函数控制台](https://console.cloud.tencent.com/scf/list)。

5. 新建云函数 - 自定义创建，函数类型选`事件函数`，部署方式选`代码部署`，运行环境选 `Python3.6`.

6. 提交方法选`本地上传文件夹`，并在下方的函数代码处上传整个项目文件夹。

7. 执行方法填写 `index.main_handler`,多用户请填写`index.main_handler_mulit`.

8. 展开高级配置，将执行超时时间修改为 `300 秒`，其他保持默认。

9. 展开触发器配置，选中自定义创建，触发周期选择`自定义触发周期`，并填写表达式`0 0 10 * * * *`（此处为每天上午 10 时运行一次，可以自行修改）

10. 完成，enjoy it！

* 阿里云
  1. 下载本项目
  2. 在脚本目录执行`pip3 install -r requirements.txt -t .`，如果无法选择`Python3.9`环境请执行`pip3 install -r requirements_qcloud.txt -t .`
  3. 在本地完整运行一次。
  4. 打开并登录[函数计算 FC](https://fcnext.console.aliyun.com/cn-hangzhou/services)。注意左上方显示的地区，可点击切换其他地区。
  5. 创建服务 （日志功能可能产生费用，建议关闭）
     1. 创建函数
     2. 从零开始创建
        1. `请求处理程序类型：处理事件请求`
        2. 推荐设置运行环境为`Python3.9`
        3. `请求处理程序：index.main_handler`，多用户请填写`index.main_handler_mulit`
        4. 配置触发器：触发器类型 定时触发器 异步调用。建议触发方式设为`指定时间`
        5. 点击创建
     3. 进入函数详情
        1. 打开函数配置
        2. 修改 `环境信息` - `执行超时时间` 为300秒。
  6. 测试运行
     1. 打开 `函数详情`
     2. 点击`测试函数`
  7. 完成

## 使用青龙面板运行（V2.12+）

### 1.拉取仓库

方式1：订阅管理

```
名称：米游社签到
类型：公开仓库
链接：https://github.com/Womsxd/AutoMihoyoBBS.git
定时类型：crontab
定时规则：2 2 28 * *
白名单：ql_main.py
依赖文件：error|mihoyo|genshin|honkai3rd|log|push|req|set|tools|con|acc|honkai2|tearsofthemis|captcha|main
```

方式2：指令拉取

```sh
ql repo https://github.com/Womsxd/AutoMihoyoBBS.git "ql_main.py" "" "error|mihoyo|genshin|honkai3rd|log|push|req|set|tools|con|acc|honkai2|tearsofthemis|captcha|main"
```

### 2.环境变量添加

在config.sh中添加

```sh
export AutoMihoyoBBS_config_path="/ql/data/config/"
```

### 3.复制配置文件

**进入容器后运行以下命令**（docker exec -it ql bash）修改ql为你的青龙容器名字

```sh
cp /ql/data/repo/Womsxd_AutoMihoyoBBS/config/config.yaml.example /ql/data/config/config.yaml
```

### 4.添加依赖

在青龙面板依赖管理中添加httpx及PyYAML

### 5.编辑配置文件

在配置文件内config.yaml中编辑信息

*注：通知配置为青龙config.sh中配置*

## 使用的第三方库

requests: [github](https://github.com/psf/requests) [pypi](https://pypi.org/project/requests/)  (当httpx无法使用时使用)

httpx: [github](https://github.com/encode/httpx) [pypi](https://pypi.org/project/httpx/)

crontab: [github](https://github.com/josiahcarlson/parse-crontab) [pypi](https://pypi.org/project/crontab/)

PyYAML: [github](https://github.com/yaml/pyyaml) [pypi](https://pypi.org/project/PyYAML/)

## 关于使用 Github Actions 运行

本项目**不支持**也**不推荐**使用`Github Actions`来每日自动执行！

也**不会**处理使用`Github Actions`执行有关的issues！

推荐使用 阿里云/腾讯云 的云函数来进行每日自动执行脚本。

## Stargazers over time

[![Stargazers over time](https://starchart.cc/Womsxd/AutoMihoyoBBS.svg)](https://starchart.cc/Womsxd/AutoMihoyoBBS)

## License

[MIT License](https://github.com/Womsxd/AutoMihoyoBBS/blob/master/LICENSE)

## 鸣谢

[JetBrains](https://jb.gg/OpenSource)

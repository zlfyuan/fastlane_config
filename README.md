<!--
 * @Author: zluof
 * @Date: 2021-12-16 16:59:38
 * @LastEditTime: 2021-12-16 17:51:52
 * @LastEditors: zluof
 * @Description: 
 * @FilePath: /fastlane_config/fastlane/README.md
-->

# 使用

1、下载该文件到项目根目录
```shell
git clone https://github.com/zlfyuan/fastlane_config.git && mv fastlane_config fastlane
```

2、在该文件内建立 .env 文件 配置以下变量
```Swift
PGY_API_KEY = ""
PGY_USER_KEY = ""
PGY_APP_KEY = ""
DINGDING_WEBHOOK = ""

XCODEPROJ = "*.xcodeproj"
WORKSPACE = "*Lazy*.xcworkspace"
SCHEME = '*'
```

3、确认配置文件无误既可执行以下命令

发布release到AppStore
```shell
fastlane ios --env env  release
```
发布development到蒲公英
```shell
fastlane ios --env env  development message:"测试内容"
```
发布ad-hoc到蒲公英
```shell
fastlane ios --env env  beta message:"测试内容"
```


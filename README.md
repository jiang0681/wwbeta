# wuwa2.4-PS教程  
## [PS官方Discord频道-Reversed Rooms](https://discord.gg/gJgefeJJ)  
欢迎来玩！  
本人不是很有时间更新教程，所以就算PS更新也可看老教程凑合用。  
但由于2.4PS的部署和原2.1PS差距较大，故重构一次。（2025.05.17）  
测试系统：win11 22H2专业版（22621.4317）  
原2.1PS链接：  

除了此教程，还有其他大佬的优秀教程也可借鉴，可自行食用：  
[sunset的博客：wuwa-ps](https://blog.sunmkt.uk/article/wuwa-ps/)  
[频道御用视频教程-from youtube](https://www.youtube.com/watch?v=rOWBm-oJYT8)  

本文可能部分借鉴了上述大佬们的经验，再次致以感谢。  

[PS来源：wickedwaifus](https://git.xeondev.com/wickedwaifus)  

我只是写了一个教程，我不是PS的作者。  
If you cannot read Chinese，please use translation softwares or plugins to help yourself.  

本教程宗旨：最清晰、最简单。  
偏口语化，像碎碎念，但仍希望读者有基本的技术经验。  

下面开始。  
## 目录：  
[1、安装必需软件和环境](#1%E3%80%81%E5%AE%89%E8%A3%85%E5%BF%85%E9%9C%80%E8%BD%AF%E4%BB%B6%E5%92%8C%E7%8E%AF%E5%A2%83)  
[2、下载游戏本体](#2%E4%B8%8B%E8%BD%BD%E6%B8%B8%E6%88%8F%E6%9C%AC%E4%BD%93)  
[3、给游戏本体打补丁](#3%E7%BB%99%E6%B8%B8%E6%88%8F%E6%9C%AC%E4%BD%93%E6%89%93%E8%A1%A5%E4%B8%81)  
[4、下载PS并配置](#4%E4%B8%8B%E8%BD%BDps%E5%B9%B6%E9%85%8D%E7%BD%AE)  
[5、启动](#5%E5%90%AF%E5%8A%A8)  
[6、其他](#6%E5%85%B6%E4%BB%96)  

## 1、安装必需软件和环境（如有可跳过）  
点击标题即可下载。  
### 1.1 [PostgreSQL](https://www.postgresql.org/download/)
建议下载16版本，安装，一路默认就行，记住安装时设定的密码。  
找到开始菜单里的pgAdmin4并打开  
（或者直接去你“安装PostgreSQL的文件夹\pgAdmin4\runtime\”下，找到pgAdmin4.exe双击打开），  
等待pgAdmin4打开后，在左边的边栏选中Servers-->PostgreSQL-->Databases，  
在“Databases”文字上右键-->create-->Database...，  
第一栏填写它的名字为（示例）`wicked_waifus_db`，这将是你的数据库名，save就行了。  

### 1.2 [rust](https://www.rust-lang.org/tools/install)
下载最新版本，安装，一路默认就行。

### 1.3 [protoc](https://github.com/protocolbuffers/protobuf/releases)
下载最新版本，解压，把`\你解压的文件夹所在位置\protoc-31.0-win64in`添加到系统环境变量的path。  
（如何添加到系统环境变量？右键“此电脑”，属性，高级系统设置，环境变量，用户的环境变量中双击“path”，新建，框里填如：`protoc-31.0-win64/bin`，确定，确定，确定）  

## 2、下载游戏本体
2.4版本后kuro官方launcher对游戏包体的下载增加了鉴权，故弃用。  
新下载器：[wuwa-downloader](https://github.com/yuhkix/wuwa-downloader)  
by: yuhkix@github  
这个下载器可能需要全局代理，也可能直链，如果下不动请切换网络。  
找release，下载最新exe，双击，出现：

```
[*] Available versions:
1. Live - OS
2. Live - CN
3. Beta - OS
4. Beta - CN
[?] Select version:
```

这里选4，回车出现：
```
[*] Fetching download configuration...
[*] Using default.config
[?] Enter download directory (Enter for current):
```
这里写你希望游戏包体（约47G）下载到的位置。  
回车后耐心等待下载完成即可。我下了约40min。  
如果有下载失败重新打开这个exe即可，他会自动识别你还缺了那些文件，重新给你下载。

## 3、给游戏本体打补丁
### 3.1 pak补丁
[点击下载2.4 pak补丁](https://git.xeondev.com/wickedwaifus/wicked-waifus-pak/releases/tag/2.4.0)  
放到  
`Wuthering Waves (Beta) Game\Client\Content\Paks` 目录下  

### 3.2 dll补丁
[点击下载2.4 dll补丁](https://git.xeondev.com/wickedwaifus/wicked-waifus-win-patch/releases/tag/2.4.0)  
解压后找到 `_\regular\wicked-waifus-win-cn_beta_2_4_0-regular.dll`，放到  
`Wuthering Waves (Beta) Game\Client\Binaries\Win64` 目录下  

### 3.3 启动补丁
[点击下载xavo95的launcher.exe](https://git.xeondev.com/xavo95/launcher/releases)  
回到  
[master/samples/](https://git.xeondev.com/ReversedRoomsMisc/process-launcher-rs/src/branch/master/samples/)  
下载ww.toml文件, 和launcher.exe一起，放到  
`Wuthering Waves (Beta) Game\Client\Binaries\Win64` 目录下。  
新建文件命名为 `run_xavo_launcher.bat`，用记事本打开，写入：

```bat
@echo off
:: 检查是否管理员
net session >nul 2>&1
if %errorlevel% neq 0 (
    echo 正在尝试以管理员权限重新运行...
    powershell -Command "Start-Process '%~f0' -Verb RunAs"
    exit
)
cd /d "F:\Wuthering Waves(Beta)\Wuthering Waves (Beta) Game\Client\Binaries\Win64"
launcher.exe
pause
```

保存即可。  
参考视频：  
https://www.youtube.com/watch?v=kLA0kONBT_s

## 4、下载PS并配置
### 4.1 下载PS
[wicked-waifus-rs](https://git.xeondev.com/wickedwaifus/wicked-waifus-rs)  
在你要接收文件的文件夹里右键-->在终端中打开，输入：

```bash
git clone --recursive https://git.xeondev.com/wickedwaifus/wicked-waifus-rs.git

```
等待主仓库克隆完成后，可以拉取推荐的自动buff分支（by:Ruuby@Discord）  
```bash
git fetch origin refs/pull/6/head:pr-6-test

```
如果没有git也可以手动下载zip包。

### 4.2 编译PS
目录下打开cmd，一次性粘贴下面的指令：

```bash
cargo build -r --bin wicked-waifus-config-server ^
             --bin wicked-waifus-hotpatch-server ^
             --bin wicked-waifus-login-server ^
             --bin wicked-waifus-gateway-server ^
             --bin wicked-waifus-game-server
```

第一次编译可能需要一点时间，如有报错请自行询问AI助手解决。  
（将这次cmd的输入内容做成一个buildPS.bat备用）

### 4.3 配置PS
目录下打开cmd，输入：

```bash
start cmd /K "target\release\wicked-waifus-config-server.exe"
start cmd /K "target\release\wicked-waifus-hotpatch-server.exe"
start cmd /K "target\release\wicked-waifus-login-server.exe"
start cmd /K "target\release\wicked-waifus-gateway-server.exe"
start cmd /K "target\release\wicked-waifus-game-server.exe"
exit
```
（将这次cmd的输入内容做成一个runPS.bat备用）
将会打开5个终端server窗口。  
第一次运行将会生成5个配置文件：

- gameserver.toml  
- gateway.toml  
- loginserver.toml  
- hotpatch.toml  
- configserver.toml  

在 `gameserver.toml`、`gateway.toml`、`loginserver.toml` 中，找到：

```toml
[database]
host = "localhost:5432"
user_name = "postgres"
password = ""
db_name = "wicked_waifus_db"
```

将密码`password`和数据库名`db_name`写为你设置的值，保存。三个文件记得都要修改。

### 4.4 启动PS
运行runPS.bat，打开五个窗口且不报错即为成功。  
如有报错建议检查数据库名称和密码是否错误，或询问AI助手。

## 5、启动
每次启动仅需双击 `runPS.bat` 后双击 `run_xavo_launcher.bat` 即可。  
PostgreSQL数据库是一个服务, 可以通关cmd管理员关启：

- 启动：`net start postgresql-x64-16`  
- 关闭：`net stop postgresql-x64-16`

## 6、其他配置
注意：如果修改了rs文件，需退出所有PS相关程序后重新编译（运行 `buildPS.bat` 即可），然后重新创建新账户登录PS后才能生效。

### 6.1 获取指定角色
打开 `wicked-waifus-rs\wicked-waifus-game-server\src\logic\role\formation.rs`  
第12行：

```rust
const DEFAULT_FORMATION: &[i32] = &[1205, 1207, 1409];
```

这些数字分别对应角色编号：1205（长离）、1207（露帕）、1409（卡提希亚）。  
编号参考：[wuwa-ids by:yuhkix@github/discord](https://github.com/yuhkix/wuwa-ids/blob/main/characters.md)

### 6.2 进入指定地图
打开 `wicked-waifus-game-server\src\logic\player\location.rs`  
第12行：

```rust
const DEFAULT_INSTANCE_ID: i32 = 8;
```

修改 i32 值即可。地图编号参考：  
[地图 JSON](https://git.xeondev.com/wickedwaifus/wicked-waifus-data/src/branch/master/BinData/AkiMap.json)  
提示：地下金库（云底藏馆）编号为902

### 6.3 没有大招？
打开 `data\assets\game-data\BinData\BaseProperty.json`，  
全文替换以下字段：

```json
"CdReduse": 10000, ==> "CdReduse": 0,
"EnergyMax": 12500, ==> "EnergyMax": 0,
"Energy": 0, ==> "Energy": 1,
```

如需修改指定角色，请搜索角色 ID（例如 `"Id": 1606`），  
在其对应位置修改数值，如：

```json
"CdReduse": 10000,   -> 改为 0
"EnergyMax": 12500,  -> 改为 0
"Energy": 0,         -> 改为 1
```

即可实现无CD大招和满能量状态。  
### 6.4 新衣服呢？
方法by: Xx-wpc@discord  
`
wicked-waifus-rs\data\assets\game-data\BinData\RoleInfo.json
`
搜索角色id后找到对应SkinId，长离和珂莱塔的SkinId的第四位0改成1即可切换成泳装。  
同理如果想换手上的武器，修改InitWeaponItemId即可。  
提示：lupa武器 21010036 小卡武器 21020056  

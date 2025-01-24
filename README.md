# 如何玩ww2.1测试私服  
(仅适用于64位windows)(2025/01/24)  
# 私服作者也做了一个视频教程，请访问Youtube：
https://www.youtube.com/watch?v=jGISWOXrZks
## 测试服来源于：https://git.xeondev.com/wickedwaifus/wicked-waifus-rs/  
## 我只是写了一个教程，我不是这个软件的作者  
## If you cannot read Chinese，please use translation softwares or plugins to help yourself.  
## 如果你在Discord里找我问这篇教程能帮你解决的问题，我一概看不见；  
## 如果有新问题欢迎来讨论
 
## 1、下载测试服启动器：    

    https://pcdownload-huoshan.aki-game.com/pcstarter/prod/starter/10008_Pa0Q0EMFxukjEqX33pF9Uyvdc8MaGPSz/G152/1.7.1.0/cDKioEuJvj9zCC9Q04iF3pNG2JBm9rhj/installer.exe
## 2、安装启动器；
## 3、下载破解的launcher.exe：
    
    https://github.com/jiang0681/wwbeta/releases/download/1/launcher.exe
## 4、用后下载的launcher.exe，替换掉刚才安装的启动器目录下的launcher.exe。
启动器目录就像“D:\Wuthering Waves(Beta)\”这种；
## 5、双击launcher.exe，打开测试服启动器，下载2.1游戏本体，大概需要30G；
## 6、下载私服搭建需要的文件：
### a)PostgreSQL
    
    https://sbp.enterprisedb.com/getfile.jsp?fileid=1259337
下载好之后安装到你知道的位置，一路默认就行，但是要记住你的密码和data文件夹在哪；  

然后在环境变量里添加你安装的SQL的文件夹\bin的位置，比如我的就是“D:\PostgreSQL\bin”。  

（如何添加？右键“此电脑”，属性，高级系统设置，环境变量，用户的环境变量中双击“path”，新建，框里填\你安装的文件夹所在位置，比如PostgreSQL\bin，确定，确定，确定）  
### b)Rust
    
    https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe
下载好后安装即可；
### c)Protoc
    
    https://github.com/protocolbuffers/protobuf/releases/download/v29.3/protoc-29.3-win64.zip
解压文件夹到一个你看着顺眼的位置，然后把"\你解压的文件夹所在位置\protoc-29.3-win64\bin"添加到环境变量的path。    

添加环境变量同上；  

### d)wicked-waifus-rs(https://git.xeondev.com/wickedwaifus/wicked-waifus-rs)
在你要接收文件的文件夹里右键-->在终端中打开，输入：
    
    git clone --recursive https://git.xeondev.com/wickedwaifus/wicked-waifus-rs.git
等待克隆完成后，记住这个文件夹的位置，等下回来要用；
## 7、找到开始菜单里的pgAdmin4并打开  
(或者直接去你“安装PostgreSQL的文件夹\pgAdmin4\runtime\”下，找到pgAdmin4.exe双击打开)，  
等待pgAdmin4打开后，在左边的边栏选中Servers-->PostgreSQL-->Databases，
在”Databases“文字上右键-->create-->Database...，
第一栏填写它的名字为wicked_waifus_db，
save就行了。
## 8、进入wicked-waifus-rs文件夹，
在空白处右键选“在终端中打开”，
输入：
    
    cargo run --bin wicked-waifus-config-server
第一次需要一点时间编译。出现大大的“WICKED WAIFUS PS"标志，
且终端没有爆红或者退出之后，不要关终端，
依然在wicked-waifus-rs文件夹内右键空白处选“在终端中打开”，
输入：
    
    cargo run --bin wicked-waifus-hotpatch-server
下面三个同理：
    
    cargo run --bin wicked-waifus-login-server
    cargo run --bin wicked-waifus-gateway-server
    cargo run --bin wicked-waifus-game-server
第一次都先跑一遍，不管能不能跑通。
如果跑不通，关注以下几个文件：
### a)gateway.toml
最下面一行应该为：
db_name = "wicked_waifus_db"
### b)loginserver.toml
最下面几行应该为：
    
    user_name = "postgres"<---------------这是默认的用户名，如果你没改就不要动
    password = "######"<----------------------这里填你自己设定的密码！！！
    db_name = "wicked_waifus_db"
### c)gameserver.toml
中间有几行应该为：
    
    user_name = "postgres"
    password = "######"
    db_name = "wicked_waifus_db"
（和上面同理）
### 注：端口如果是默认的话都是5432，除非你自己改成了别的，需要自己去修改所有toml文件里的端口号。
### 如果还是跑不通，去【第6步a)】的data文件夹那里（你安装PostgreSQL的位置\data\），关注以下文件：
### a)pg_hba.conf
最后面几行是你的数据库的验证方式，
如果scram-sha-256（哈希）不行，把 所有 的scram-sha-256
换成md5（密码明码），如果还是不行，
换成trust（不要密码）
### b)postgresql.conf
找到752行~756行，如果不是以下的，直接改成和以下一模一样的：
    
    lc_messages = 'en_US.UTF-8' # locale for system error message
					# strings
    lc_monetary = 'en_US.UTF-8' # locale for monetary formatting
    lc_numeric = 'en_US.UTF-8' # locale for number formatting
    lc_time = 'en_US.UTF-8' # locale for time formatting

### 注意：每次修改文件后，都需要到服务里重启PostgreSQL服务。  

（如何重启服务？win+r，输入services.msc，回车，找到“postgresql-x64-17 - PostgreSQL Server 17”服务，右键-->重新启动）  

## =====================分界线=============================
如果你五个终端都跑通了，没有爆红，没有异常退出，再接着往下！
## 9、下载.pak文件
    
    https://git.xeondev.com/wickedwaifus/wicked-waifus-pak/releases/download/2.1.0/rr_fixes_100_p.pak
放在"\Wuthering Waves(Beta)\Wuthering Waves Game\Client\Content\Paks\"目录下
## 10、下载.dll文件
    
    https://github.com/jiang0681/wwbeta/releases/download/1/CrashSight64.dll
放在“\Wuthering Waves(Beta)\Wuthering Waves Game\Client\Binaries\Win64\"目录下，覆盖掉原来的CrashSight64.dll文件！！！
## 11、有两种防崩溃的方法
（这一步如果你不做你的游戏会光速崩溃）  

两种方法任选其一即可
### a)下载winhttp.dll和libraries.txt文件
    
    https://github.com/jiang0681/wwbeta/releases/download/1/winhttp.dll
和
    
    https://github.com/jiang0681/wwbeta/releases/download/1/libraries.txt
和CrashSight64.dll一起，放在“\Wuthering Waves(Beta)\Wuthering Waves Game\Client\Binaries\Win64\"目录下
### b)用xavo95的launcher.exe  
下载地址：

    https://git.xeondev.com/xavo95/launcher/releases
参考视频：
    
    https://www.youtube.com/watch?v=kLA0kONBT_s
## 12、在五个终端和数据库开着的情况下，
双击“\Wuthering Waves(Beta)\Wuthering Waves Game\Client\Binaries\Win64\"目录下的Client-Win64-Shipping.exe，开始游戏，进去的登录曲变了你就成功了。
进去后新创角色啥的功能你就自己探索了。  

## 13、怎么快速启动那五个cargo的服务
(来自@Castorice的建议)
每次打开五个服务都很麻烦。  
如果你已经配置好了并且多次运行都可以成功，可以在wicked-waifus-rs的文件夹里新建一个runall.bat文件，  
用记事本写入：  
    
    @echo off
    start cmd /K "cargo run -r --bin wicked-waifus-config-server"
    start cmd /K "cargo run -r --bin wicked-waifus-hotpatch-server"
    start cmd /K "cargo run -r --bin wicked-waifus-login-server"
    start cmd /K "cargo run -r --bin wicked-waifus-gateway-server"
    start cmd /K "cargo run -r --bin wicked-waifus-game-server"
    exit
保存好后下次直接双击这个runall.bat文件就可以同时秒开5个cargo的服务了！

# 2025/01/24教程新增：获取指定角色和进入指定副本地图方法
## 14、获取指定角色
找到\wicked-waifus-rs\wicked-waifus-game-server\src\logic\player\mod.rs文件，第135行，

    let formation = vec![1506, 1206, 1606];
这里的三个四位数字分别代表你进去后队伍里的三个角色的唯一编号。  

但是直接修改是不够的。应遵循以下步骤：
### 0、退出游戏，退出所有5个cmd终端
### a)在mod.rs文件里，将三个数字修改为你想要的三个角色的编号。  
（角色及对应编号在
    
    https://git.xeondev.com/wickedwaifus/wicked-waifus-data/src/branch/master/BinData/RoleInfo.json
里找。 ） 

提示：看到文件后可以发现第一个id：1402是秧秧，其他角色的对应方法自己类比，没办法细讲；  

修改好后记得保存文件；
### b)打开你的pgAdmin4(前面有讲)，删除你的wicked_waifus_db数据库，然后再重新建一个一样的  
(实际上不必要删除这个，但是这样最省事)
### c)重新运行那5个cargo指令，应该会重新编译的所以时间稍微长一点  
### d)进入游戏，点击右下角的新建一个角色账户，此时角色应该就是你改的那3个了。  
如果不是的话，你失败了，重新看一遍你的过程重新来。
## 15、进入指定地图
找到\wicked-waifus-rs\wicked-waifus-game-server\src\logic\player\location.rs文件，第12行，

    const DEFAULT_INSTANCE_ID: i32 = 8;
这个i32等于多少你就会进入到这个编号代表的哪个地图里。当然了也不是直接改就能生效的。  

地图及其对应编号在

    https://git.xeondev.com/wickedwaifus/wicked-waifus-data/src/branch/master/BinData/AkiMap.json
里。

提示：地下金库（云底藏馆）对应编号是902; 

修改好 i32=几几几 之后，重复你改角色用的0、b)、c)、d)、这几个步骤，进入游戏就直接是你想要进入的地图了。
## 16、其他功能不妨直接来Discord里问作者  

我只是一个搬砖的（  

https://discord.gg/626PmU85bS

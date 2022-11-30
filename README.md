# Bedhock
Bedrock Server Hooking Tool

This tool was made for getting better experience in Minelab (which is a server I play with friends during my livestreams).

Tested on 1.19.10



```bash
#安装依赖
sudo apt update
sudo apt install build-essential cmake libzmq3-dev libczmq-dev libjsoncpp-dev


#设置默认分支
git branch --set-upstream-to=origin/main master

#克隆子模块
git pull --recurse-submodules

#编译项目

cmake CMakeLists.txt -S./ -B./build -G "Unix Makefiles"
cd build
make


# 编译成功后得到 libBedhock.so 复制到 bedrock_server 目录


#启动方法
LD_PRELOAD=./libBedhock.so LD_LIBRARY_PATH=. ./bedrock_server

```



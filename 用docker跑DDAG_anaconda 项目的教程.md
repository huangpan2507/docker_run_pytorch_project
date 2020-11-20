#  用docker跑DDAG_anaconda 项目的教程

###　第一步：在装有docker的电脑里 将docker  image 从docker hub下载，终端输入如下命令：        

> **1. docker pull huangpan2507/ddag_reid:v0.2**                *注释：docker pull   仓库名：标签*
>
> 



### 第二步：查看电脑已有镜像image

> **2. docker images**                                                                   *注释：若提示权限，则在前面加 sudo*



### 第三步：挂载放有数据库及代码的U盘，将DDAG-master目录里面的所有文件拿出来放  与 数据集均处于U盘下！

就是 数据集与所有 DDAG-master里面的文件在U盘路径下。

> **3.1  sudo fdisk  -l **             *注释 :系统硬盘和分区情况. 注意后面的为小写的l此时返回的结果会知道U盘的 名字为，比如sdb1*。或者使用 **lsblk**
>
> **3.2  sudo mount  /dev/u盘名字   /mnt**     :然后回车。将U盘挂载到了mnt下面



### 第四步：在终端输入如下命令，运行下载的镜像：   将会进入anaconda的base环境                                                                

> **4. sudo docker run -it --shm-size="11g" --gpus=all -e  NVIDIA_DRIVER_CAPABILITIES=compute,utility -e NVIDIA_VISIBLE_DEVICES=all  -p 10.160.76.82:443:8888 -v  /mnt/:/mnt/ huangpan2507/ddag_reid:v0.2    /bin/bash**
>
> *注释 :*
>
> 上面是一条命令。你需要 ***需改的地方*** 就是 ***10.160.76.82***(我的) ：改为自己电脑的IP地址。***其余不需要变动！***
>
> 1. 端口号及8888都原封不动就可以了。 **10.160.76.82:443:8888**     格式为：  自己IP:端口号:8888
>
> 2. --shm-size="11g" 为设置docker 共享内存大小，此处设为11G，默认内存大小为64M
>
> 3. -e  NVIDIA_DRIVER_CAPABILITIES=compute,utility -e NVIDIA_VISIBLE_DEVICES=all：使用nvidia-docker，用上GPU
>
> 4. -v  /mnt/:/mnt/ ：为将本地存放数据集及代码目录挂载到docker 容器上，两个路径均要以 / 开头
>
> ​      格式：-v   /本地数据集及代码目录路径/：/docker 容器路径，如不存在则会自动新建.
>
> ​       因为拷贝过去代码中数据库的路径为：/mnt/RegDB/    /mnt/SYSU-MM01/   



### 第五步：显示镜像里面的虚拟环境并进入

> **5.1  conda env list**                                        *注释：显示虚拟环境*
>
> **5.2  conda activate DDAG_2**                      *注释：进入刚刚显示的虚拟环境，该名字不用改*
>
> **5.3  cd mnt**                                                    *注释：进入挂载的docker容器目录下，该路径存有数据集和代码*



### 第六步：安装tensorboardX ，这个是镜像漏了一个包

> **6. pip install tensorboardX**                   *注释：安装该包*



### 第七步： 运行代码

> **7.  python train_ddag.py --dataset sysu --lr 0.1 --graph --wpa --part 3 --gpu 0**
>
> 
>
> 注释：该项目所在github地址：https://github.com/mangye16/DDAG  

完结！！

`停止的话：docker stop容器ID或容器名称`停止容器(***也就是@符号后面的一大串数字***)



docker 常用操作： https://blog.csdn.net/lizhiqiang1217/article/details/89070075?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.add_param_isCf&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.add_param_isCf

https://blog.csdn.net/qq_33820379/article/details/81185361




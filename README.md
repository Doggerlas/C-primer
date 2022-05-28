# 代码升级指令
tf_upgrade_v2 \
  --intree SketchCNN/ \
  --outtree SketchCNN_v2/ \
  --reportfile report.txt
 
# 训练指令
    python train_naiveNet.py --dbTrain=../sampleData/train --dbEval=../sampleData/eval --outDir=../output/train_naiveNet --nb_gpus=2 --devices=0,1 --lossId=0
 
# docker相关指令
##### 启动docker(因为没有root 所以不用管)
    systemctl start docker         
##### 查看nvidia-docker安装情况(已经有人安装完了)   
    apt show nvidia-docker2 
##### 创建一个可以使用显卡nvidia_docker的容器(这个需要之前有人装过nvidia-docker)
    nvidia-docker run -it --name first_container nvidia/cuda:11.0-base /bin/bash
##### 删除指定的容器    
    docker rm 482ee69cb441     
##### 启动容器
    docker start 482ee69cb441   
##### 关闭容器
    docker stop 482ee69cb441   
##### 进入容器
    docker attach 482ee69cb441         
##### 查看正在运行的容器
    docker ps      
##### 列出所有容器
    docker ps -a            
##### 退出容器    
    exit                               
##### 显示完全id：482ee69cb4415710228d1e64fdf10fb572a86b4fcbdf9924868746457c2b4c6b
	  docker inspect --format="{{.Id}}"  first_container      
##### 主机到docker文件传输
    docker cp /home/user1/Wlm/Anaconda3-5.3.0-Linux-x86_64.sh 482ee69cb4415710228d1e64fdf10fb572a86b4fcbdf9924868746457c2b4c6b:/home   
##### docker到主机文件传输
    docker cp 482ee69cb4415710228d1e64fdf10fb572a86b4fcbdf9924868746457c2b4c6b:/home/SketchCNN/report.txt /home/user1
##### 我的docker信息
    CONTAINER ID   IMAGE                   COMMAND       CREATED        STATUS                         PORTS     NAMES
    482ee69cb441   nvidia/cuda:11.0-base   "/bin/bash"   2 hours ago    Up About an hour                         first_container
# conda相关指令
##### 检查conda版本:
    conda --version
##### 升级当前版本的conda
    conda update conda
##### 创建并激活一个python3.6的环境snowflake
    conda create --name snowflake biopython
##### 激活环境
    source activate snowflakes
##### 列出所有环境
    conda info -e
##### 切换到另一个环境
    source activate snowflakes
##### 从你当前工作环境的路径切换到系统根目录
    source deactivate
##### 通过克隆snowfllakes来创建一个称为flowers的副本
    conda create -n flowers --clone snowflakes
##### 删除一个环境
    conda remove -n flowers 
##### 假设你需要python3来编译程序，但是你不想覆盖掉你的python2.7来升级，你可以创建并激活一个名为snakes的环境，并通过下面的命令来安装最新版本的python3：
    conda create -n snakes python=3 然后检查新的环境中的python版本 python --version
##### 查看已安装包
    conda list
##### 向bunnies环境安装beautifulsoup4包
    conda install --name bunnies beautifulsoup4或者 先激活环境activate bunnies 再安装包conda install beautifulsoup4
##### 通过pip命令来安装包
    先激活source activate bunnies在安装pip install see    

# 20220528 在482ee69cb441容器中
## 1.为了使用GPU新建一个nvidia_docker用户first_container  
##### 创建一个可以使用显卡nvidia_docker的容器
    nvidia-docker run -it --name first_container nvidia/cuda:11.0-base /bin/bash
##### 查看完全id
    docker inspect --format="{{.Id}}"  first_container           #482ee69cb4415710228d1e64fdf10fb572a86b4fcbdf9924868746457c2b4c6b
##### 复制anaconda.sh到容器
    docker cp /home/user1/Wlm/Anaconda3-5.3.0-Linux-x86_64.sh 482ee69cb4415710228d1e64fdf10fb572a86b4fcbdf9924868746457c2b4c6b:/home


## 2.安装vi
	apt-get update
	apt-get upgrade
	apt-get install vim
	
	
## 3.安装Anaconda
##### 下载版本 
	Anaconda3-5.3.0-Linux-x86_64.sh	636.9M	2018-09-27 16:01:35	4321e9389b648b5a02824d4473cfdbf	
##### 安装
	bash Anaconda3-5.3.0-Linux-x86_64.sh 安装位置/root/anaconda3
##### 配置
	vi ~/.bashrc 添加环境变量	export PATH="/root/anaconda3/bin:$PATH" 然后source ~/.bashrc
##### 查看版本
	conda --version或conda -V
##### 打开jupyter(暂时用不到)
	jupyter notebook  --allow-root
	
## 4.anaconda3安装TensorFlow2.6.0gpu
##### 知道上述方法不行，看了主机支持GPU和conda 我想升级conda后再进行布置tf2.6.0环境
	conda update conda									#升级到最新版4.13.0
##### 创建环境
	conda create -n tensorfolw2.6.0_gpu python=3.8.8
	conda info -e
##### 进入环境
	conda activate tensorfolw2.6.0_gpu
	conda list
	python -V
##### 安装tensorfolw2.6.0_gpu
	pip install tensorflow-gpu==2.6.0 -i https://pypi.tuna.tsinghua.edu.cn/simple   
###### 降级protobuf，避免出现问题：TypeError: Descriptors cannot not be created directly
	pip install protobuf==3.20.1
##### 安装cudatoolkit和cudnn 避免出现链接库.so找不到的问题
	conda install cudatoolkit=11 
	conda install cudnn
##### 以下在python3打开
		import tensorflow as tf
		tf.test.is_built_with_cuda()	#检查tensorflow是否得到CUDA支持，安装成功则显示true，否则为false
		tf.test.is_gpu_available()		#检查tensorflow是否可以获取到GPU，安装成功则显示true，否则为false
	
###### 卸载指令(不是版本问题不要卸载)
	pip uninstall tensorflow tensorflow-gpu
	




	

	

---
layout:     post
title:      Anaconda configuration notes
subtitle:   
date:       2020-04-12
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - Anaconda
 - System
 - Configuration
---
2019-04-03:
10:00pm-01:00am


1.Ubuntu安装pycharm

        1.解压压缩包
        2.cd到解压目录     terminal $ /.pycharm.sh
        3.根据提示完成安装
        
2. pycharm固定到任务栏（失败）
3. 将pycharm固定到桌面
        
        1.智障方法（我最后才知道）：
            pycharm启动-->菜单栏tools-->create desktop entry
            
        2.https://blog.csdn.net/jpch89/article/details/81739176（很详细）
        
4.安装anaconda

5.分清 anaconda和系统自带python两个环境
        
        安装anaconda后如何往原python环境中安装包
            https://blog.csdn.net/jianyuchen23/article/details/78965630
            安装Anaconda后，如果使用了其自动改变环境变量，那么默认的python ,pip都将变为Anaconda下的

                这时如果想切换默认python为原始独立python, 
                1. sudo gedit ~/.bashrc 
                2. 添加 alias python=’/usr/bin/python2.7’ 
                3.source ~/.bashrc

                同理 切换回Anaconda下,就改变python的位置

                使用原始python 的pip安装模块到其环境下可以使用 
                sudo /usr/bin/pip install **

        如何在已安装Python条件下，安装Anaconda,，并将原有Python添加到Anaconda中
            ：https://www.cnblogs.com/yamin/p/7111397.html


2019-04-04：

1.Anaconda安装后带的软件（win下）：
            
           ANACONDA  Navigator 管理工具包和环境的图形用户界面
           Jupyter notebook ：基于web的交互式计算环境，可以编辑易于人们阅读的文档，用于展示数据分析的过程。
           qtconsole：一个可执行 IPython 的仿终端图形界面程序，相比 Python Shell 界面，qtconsole 
           spyder：一个使用Python语言、跨平台的、科学运算集成开发环境。可以直接显示代码生成的图形，实现多行代码输入执行，以及内置许多有用的功能和函数。
           
           安装完成后，我们还需要对所有工具包进行升级，以避免可能发生的错误。打开你电脑的终端，在Anaconda Prompt中输入：

            conda upgrade --all
            
            
1.Anaconda 基本命令
           
   管理python包
           
           安装一个 package：

            conda install package_name
            这里 package_name 是需要安装包的名称。你也可以同时安装多个包，比如同时安装numpy 、scipy 和 pandas，则执行如下命令：

            conda install numpy scipy pandas
            你也可以指定安装的版本，比如安装 1.1 版本的 numpy ：

            conda install numpy=1.10
            移除一个 package：

            conda remove package_name
            升级 package 版本：

            conda update package_name
            查看所有的 packages：

            conda list
            如果你记不清 package 的具体名称，也可以进行模糊查询：

            conda search search_term
            
            
   如何管理Python环境？
        默认的环境是 root，你也可以创建一个新环境：

        conda create -n env_name list of packages
        其中 -n 代表 name，env_name 是需要创建的环境名称，list of packages 则是列出在新环境中需要安装的工具包。

        例如，当我安装了 Python3 版本的 Anaconda 后，默认的 root 环境自然是 Python3，但是我还需要创建一个 Python 2 的环境来运行旧版本的 Python 代码，最好还安装了 pandas 包，于是我们运行以下命令来创建：

        conda create -n py2 python=2.7 pandas
        细心的你一定会发现，py2 环境中不仅安装了 pandas，还安装了 numpy 等一系列 packages，这就是使用 conda 的方便之处，它会自动为你安装相应的依赖包，而不需要你一个个手动安装。

        进入名为 env_name 的环境：

        source activate env_name
        退出当前环境：

        source deactivate
        另外注意，在 Windows 系统中，使用 activate env_name 和 deactivate 来进入和退出某个环境。

        删除名为 env_name 的环境：

        conda env remove -n env_name
        
        显示所有的环境：

        conda list
        当分享代码的时候，同时也需要将运行环境分享给大家，执行如下命令可以将当前环境下的 package 信息存入名为 environment 的 YAML 文件中。

        conda env export > environment.yaml
        同样，当执行他人的代码时，也需要配置相应的环境。这时你可以用对方分享的 YAML 文件来创建一摸一样的运行环境。

        conda env create -f environment.yaml
        常用操作

        # 创建一个名为python27的环境，指定Python版本是2.7（不用管是2.7.x，conda会为我们自动寻找2.7.x中的最新版本）
        conda create --name python27 python=2.7
        # 安装好后，使用activate激活某个环境
        activate python27
        # 激活后，会发现terminal输入的地方多了python27的字样，实际上，此时系统做的事情就是把默认3.5环境从PATH中去除，再把2.7对应的命令加入PATH
        # 此时，再次输入
        python --version
        # 可以得到`Python 2.7.5 :: Anaconda 4.2.0 (64-bit)`，即系统已经切换到了2.7的环境
        # 如果想返回默认的python 3.5环境，运行
        deactivate python34 
        # 删除一个已有的环境
        conda remove --name python34 --all
        
        
        
19-04-09:
       安装anaconda后，打开terminal会默认是（base）环境，因为
       anaconda命令被添加进了～/.bashrc文件。
       
       conda deactivate 可以退出
       
       conda info --env 查看所有python环境，*代表当前环境
       
       conda activate pytorch    进入pytorch环境
       

            
       This can also be because auto_activate_base is set to True. You can check this using the following command

conda config --show | grep auto_activate_base
To set it false

conda config --set auto_activate_base False


使用 pip3 install 安装的目录是:
```
(pytorch) runfeng@runfeng-pc:~$ pip3 show imageai
Name: imageai
Version: 2.0.2
Summary: A flexible Computer Vision and Deep Learning library for applications and systems.
Home-page: https://moses.specpal.science
Author: Moses Olafenwa and John Olafenwa
Author-email: UNKNOWN
License: MIT
Location: /home/runfeng/.local/lib/python3.6/site-packages
Requires: 
```

是系统自带python解释器的目录


想安装在anaconda目前环境的目录下，需要是pip install



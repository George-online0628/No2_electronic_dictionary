电子字典

功能说明：
１．用户可以登录和注册
　　　登录凭借用户名密码即可
　　　注册要求用户必须填写用户名和密码其他内容自定
　　　用户名要求不能够重复
２．用户数据要求使用数据库长期保存
３．能够满足多个用户同时登录操作的需求
４．功能分为客户端和服务端，客户端主要发起请求，服务端处理请求，
　　　用户启动客户端立即进入一级界面（登录　　注册　　退出）
５．用户登录后即进入二级界面（查单词　　查看历史记录　　退出）
　　　　单词本：每行一个单词　　单词和解释之间一定有空格　　后面的单词一定比前面的大
　　　　查单词：输入单词，显示单词意思，可以循环查询．输入##表示退出查询
　　　　查看历史记录：查看当前用户的历史查询记录(name,word,time)
    退出：退出到一级界面，相当于注销

技术点：
　　　　什么并发，什么套接字，什么数据库？文件处理还是数据库查询？如何将单词存入数据库？
　　　　>>>进程并发os.fork()，流套接字 tcp套接字，MySQL,文件处理

建立数据表：
　　　１．共建立３张表，分别是：用户信息，历史记录　，存单词
　　　２．用户信息：注册，登录
　　　　　　历史记录：查历史记录，查单词
　　　　　　存单词：查单词

项目分析：
服务器：登录　　注册　　查词　　历史记录
客户端：打印界面　　发出请求　　接收反馈　　打印结果


工作流程：创建数据库，存储数据　-->> 搭建通信框架，建立并发关系　-->> 实现具体功能封装
1.创建数据库，存储数据
    user:id  name  passwd
    hist:id  name  word  time
    words:id word  interpret

2.搭建基本框架
服务器：　创建套接字 --->> 创建父子进程　--->> 子进程等待处理客户端请求　
　　　　　　　　--->> 父进程继续接收下一个客户端连接

客户端：　创建套接字　--->> 发起连接请求 --->> 一级界面　--->> 请求(注册，登录，退出)
        --->>登录成功，进入二级界面　--->> 请求(查询，历史记录)

3.功能实现
注册
　　　客户端
　　　　　　　1.输入注册信息　2.将注册信息发送给服务器　3.得到服务器反馈
　　　服务端
　　　　　　　1.接收请求　2.判断是否允许注册　3.将结果反馈给客户端　4.注册信息插入数据库

登录
　　　客户端
　　　　　　　1.输入登录信息 2.将登录信息发送给服务器 3.得到服务器反馈 4.确认可以进入第二界面
　　　服务端
　　　　　　　1.接收请求 2. 判断信息是否正确 3.将信息反馈给客户端 

查词
　　　客户端：
　　　　　　　1.输入要查的单词,当输入##代表退出查词　2.　将信息发送给服务器 3.得到服务器反馈 　　　　　　　4.显示查找的内容
   服务端: 　　　　
   　　　1.接收请求　2.数据库中查找单词,可用文本查询　3.　将结果反馈给客户端　4．将信息存入
   　　　　数据库的历史记录表中，可以写在函数中


历史记录
    客户端: 1.点击按钮 2.将信息发给服务器 3. 得到服务器反馈 　　　　　　　4.判断如果内容为空，如果内容不为空，显示历史记录内容
    服务端: 1.接收请求 2.打开hist数据库　3.将搜索的数据反馈给客户端 


    电子词典开发流程：
    １．根据需求设计框架
    ２．指定工作流程
    ３．完成技术点总结和设计库
    ４．完成数据设计
    ５．搭建通信框架
    ６．完成具体功能设计
    ７．编码实现
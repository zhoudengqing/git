git安装完成后要配置用户名和邮件地址：
  git config --global user.name "zhoudengqing"
  git config --global user.email "1006783171@qq.com"
配置完成后可以通过git config --list查看配置信息：
  root@ubuntu:~# git config --list
  user.name=zhoudengqing
  user.email=1006783171@qq.com
--global指定了配置信息作用与整个系统，如果不针对整个系统，可以不加--global选项。





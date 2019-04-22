##Git常用命令

cd 进入/切换
cd .. 返回上级
ls 当前目录下的文件列表（查看当前文件夹下的文件）
pwd 当前目录路径
mkdir 创建目录（创建文件夹）
touch 创建文件
rm 删除文件 -d 删除空目录 -rf 强制删除

———————————————————————

2.ssh-key注册
命令:ssh-keygen 三次回车
vim ~/.ssh/id_rsa.pub 打开编辑公用钥匙，复制粘帖即可

———————————————————————

3.git 常用命令
git status 查看当前git 状态
git 检查git
git --version 检查git当前版本
git branch 查看当前所在分支
git branch xxx 创建xxx分支
git checkout xxx 切换到xxx分支
git checkout -b xxx 简化写法，创建xxx,并切换到xxx分支
git init 初始化git仓库
ls -ah 显示当前隐藏文件
git diff 当前文件修改内容
git log 提交版本历史
git log --pretty=oneline 提交版本历史 显示一行
rm 删除
git add 把文件添加到暂存区
git commit 把暂存区提交到仓库 都是在本地
git reset --hard HEAD^ | commit ID 回退版本
git pull 查看分支更新,多人协作的时候，你的代码不一定是最新的，所以切换到master上的时候，最后git pull一下，把最新的提交代码pull下来。

3.远程仓库 github为例
克隆远程仓库到本地：git clone git@github.yourName/xxx.git 自定义名称
本地git全局设置，目的是让git知道是不是你本人提交：
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱"

cd 进入到你克隆的仓库，进行第一次修改
mkdir test //创建test 目录
cd test 进入test 目录
git init 初始化git仓库
touch README 创建readme文件
git add README
git commit -m 'first commit'

和远程仓库进行关联,并进行第一次推送：
git remote add origin git@github.yourName/xxx.git
git push -u origin master

---------------规范-------------------
git的默认分支是master分支，可以用 git branch 命令查看，master分支建议是和远程仓库一致，保持是推送的代码，工作的分支可以另外创建：
git branch dev
git checkout dev
在dev分支上开发，在dev上开发完成后，add commit。 切换到master分支：git checkout master
在master分支上合并dev上的代码：git merge dev
推送代码：git push origin master
### 相对位置计算符号~ ^
记录/分支名/HEAD ~/^num   
~num 表示距离目标num处的提交记录
^num 表示目标的第num父级提交记录(目标的直接父级永远是1，然后其他合并分支按照左右顺序递加)

### checkout
git checkout 位置(分支名/HEAD~num^num)   
将HEAD移动到指定位置，如果位置是一个分支名，则选中该分支

### commit
git commit
在HEAD所在位置提交记录，如果HEAD在一个分支名，则在该分支提交记录

### branch
git branch 分支名
在HEAD所在位置所在位置创建指定分支   
参数 -f 用来移动分支名   
git branch -f 分支名 位置(分支名/HEAD~num^num)
将分支名移动到指定位置

### tag
git tag 标签名 提交记录  
设置提交记录的标签命名，不指定提交记录，则设置HEAD的标签命名。
ps 标签名相当于一个重要版本，在查找提交记录时作为相对位置

### describe
git describe 提交记录   
查询提交记录(没填则是当前HEAD位置)与最近标签之间的提交记录情况，结果如下：   
```javascript
<tag>_<numCommits>_g<hash> 
```
tag 表示的是离 查询的目标 最近的标签， numCommits 是表示这个 目标 与 tag 相差有多少个提交记录， hash 表示的是你所给定的 目标的哈希值 

### reset 
git reset 位置(分支名/HEAD~num)   
将当前选中分支本地回退到指定位置，当前选中分支会转移到目标位置
### revert 
git revert 位置(分支名/HEAD~num)   
将当前选中分支提交回退到指定记录，在当前选中分支会多一个目标位置的提交  
注意：
回退都要使当前HEAD处于目标分支即需要预先运行git checkout 分支名

### cherry-pick
git cherry-pick <提交记录>...   
将提交记录按照顺序添加到当前HEAD所在的分支上，需要知道提交记录的位置，可以是哈希、标签、分支名，但都只将当前所处的一个记录提交；会更新分支的标签位置
### rebase
git rebase 分支名 位置(分支名/HEAD~num^num)   
将分支名(不填分支则选择当前HEAD)与目标位置之间的的不同所有提交记录添加到目标位置(不填分支则选择当前HEAD)后面。   
参数 -i 用来弹出窗口列出所有记录供选择，可以调整顺序和忽略




### clone
git clone   
将整个仓库clone到本地，每个分支都会创建一个对应的远程分支记录本地分支对应远程分支的状态，名称为仓库名/分支名   
例如：   
仓库为origin ，有两个分支master，foo，git clone操作后，本地会完整拷贝仓库信息，同时本地会给master生成一个远程分支origin/master记录本地相对远程分支的状态，master记录本体修改提交状态，当master和origin/master则表示本地相对远程没有修改，对foo同理生成origin/foo

注意：   
以上是自动生成的对应master，origin/master，也可以修改对应关系   
1. git checkout -b tmaster o/master；
创建一个名为tmaster的分支，它跟踪本地远程分支 o/master，这之后 pull 和push只会更新本地的 tmaster和o/master，不会更新master
2. git branch -u o/master foo；foo就会跟踪 o/master了

### fetch
1. git fetch <remote> <place>   
remote仓库名，place分支名，只会更新place分支
2. git fetch origin <source>:<destination>   
将source处的提交更新到到destination分支上，origin仓库名   
注意：git fetch origin :bar，本地新建bar分支
3. git fetch   
将仓库的所有提交都更新到本地，所有分支

### pull
1. git pull   
等价于git fetch+ git merge    
拉取当前(HEAD选中的(未选中需要提前checkout选中))分支的最新内容 ，并会将本地分支和远程分支merge合并(如o/master 和master)
2. git pull --rebase   
等价于git fetch+ git rebase   将HEAD当前选中的分支(未选中需要提前checkout选中)与对应的远程分支(更到最新)rebase合并

### push
1. git push <remote> <place>   
remote仓库名，place分支名，将本地place分支与远程仓库的place对比，提交上所有未提交的，如果remote不填就默认使用HEAD，但HEAD不是分支名，除非HEAD在一个分支名处，就使用该分支名；只提交place分支
2. git push origin <source>:<destination>   
将source处的提交添加到destination分支上，如果destination分支不存在则自动创建   
注意：git push origin :foo，删除远程仓库上的foo分支,可以理解为提交了一个空到foo，foo就被删除  
3. git push   
将HEAD当前选中的分支的修改提交远程仓库

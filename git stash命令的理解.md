## git stash
```
git stash
```
git stash这个命令可以将当前的工作状态保存到git栈，在需要的时候再恢复

### 1.1 git stash 
```
git stash 
```
保存当前的工作区与暂存区的状态，把当前的工作隐藏起来，等以后需要的时候再恢复，git stash 这个命令可以多次使用，每次使用都会新加一个stash@{num}，num是编号

### 1.2 git stash pop

```
git stash pop
```
默认恢复git栈中最新的一个stash@{num}，建议在git栈中只有一条的时候使用，以免混乱
### 1.3 git stash list

```
 git stash list
```
查看所有被隐藏的文件列表
### 1.4 git stash apply

```
git stash apply
```
恢复被隐藏的文件，但是git栈中的这个不删除，用法：git stash apply stash@{0}，如果我们在git stash apply 的时候工作目录下的文件一部分已经加入了暂存区，部分文件没有，
当我们执行git stash apply之后发现所有的文件都变成了未暂存的，如果想维持原来的样子，即暂存过的依旧是暂存状态，那么可以使用 git stash apply --index
### 1.5 git stash drop

```
git stash drop
```
删除指定的一个进度，默认删除最新的进度，使用方法如git stash drop stash@{0}
### 1.6 git stash clear 

```
 git stash clear 
```
删除所有存储的进度
### 1.7 git stash show
```
git stash show
```
显示stash的内容具体是什么，使用方法如 git stash show stash@{0}
### 1.8 查看帮助

```
git stash --help
```

# 远程仓库改变，同步本地

1、查看远程仓库

```shell
git remote -v
```

2、把远程库更新到本地

```shell
git fetch origin master
```

3、比较远程更新和本地版本库的差异

```shell
git log master.. origin/master
```

4、合并远程库

```shell
git merge origin/master
```

# 本地改变，推到远程

1、查看文件修改情况

```shell
git status
```

2、添加文件(添加所有文件是“ . “)

```shell
git add .
```

3、提交文件(xxx为提交说明)

```shell
git commit -m "xxx"
```

4、推文件（同步到远程）

```shell
git push
```


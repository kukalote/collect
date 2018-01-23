# git 子模块常用命令

### 下载项目及子模块代码
```bash
$ git clone --recursive git://github.com/foo/bar.git
```

### 在已有项目中更下载更新子模块代码
```bash
$ git clone git://github.com/foo/bar.git
$ cd bar
$ git submodule update --init --recursive
```

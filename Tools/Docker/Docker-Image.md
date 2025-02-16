镜像作为 Docker 三大核心概念中，最重要的一个关键词，它有很多操作，是您想学习容器技术不得不掌握的。

### 目录

<!-- TOC -->

- [一 Docker 下载镜像](#一-docker-下载镜像)
    - [1.1 下载镜像](#11-下载镜像)
    - [1.2 验证](#12-验证)
    - [1.3 下载镜像相关细节](#13-下载镜像相关细节)
    - [1.4 PULL 子命令](#14-pull-子命令)
- [二 Docker 查看镜像信息](#二-docker-查看镜像信息)
    - [2.1 images 命令列出镜像](#21-images-命令列出镜像)
    - [2.2 使用 tag 命令为镜像添加标签](#22-使用-tag-命令为镜像添加标签)
    - [2.3 使用 inspect 命令查看镜像详细信息](#23-使用-inspect-命令查看镜像详细信息)
    - [2.4 使用 history 命令查看镜像历史](#24-使用-history-命令查看镜像历史)
- [三 Docker 搜索镜像](#三-docker-搜索镜像)
    - [3.1 search 命令](#31-search-命令)
    - [3.2 search 子命令](#32-search-子命令)
- [四 Docker 删除镜像](#四-docker-删除镜像)
    - [4.1 通过标签删除镜像](#41-通过标签删除镜像)
    - [4.2 通过 ID 删除镜像](#42-通过-id-删除镜像)
    - [4.3 删除镜像的限制](#43-删除镜像的限制)
    - [4.4 清理镜像](#44-清理镜像)
- [五 Docker 创建镜像](#五-docker-创建镜像)
    - [5.1 基于已有的镜像创建](#51-基于已有的镜像创建)
    - [5.2 基于 Dockerfile 创建](#52-基于-dockerfile-创建)
- [六 Docker 导出&加载镜像](#六-docker-导出加载镜像)
    - [6.1 导出镜像](#61-导出镜像)
    - [6.2 加载镜像](#62-加载镜像)
- [七 Docker 上传镜像](#七-docker-上传镜像)
    - [7.1 获取 Docker ID](#71-获取-docker-id)
    - [7.2 创建镜像仓库](#72-创建镜像仓库)
    - [7.3 上传镜像](#73-上传镜像)
- [八 总结](#八-总结)

<!-- /TOC -->

## 一 Docker 下载镜像

如果我们想要在本地运行容器，就必须保证本地存在对应的镜像。所以，第一步，我们需要下载镜像。当我们尝试下载镜像时，Docker 会尝试先从默认的镜像仓库（默认使用 Docker Hub 公共仓库）去下载，当然了，用户也可以自定义配置想要下载的镜像仓库。

### 1.1 下载镜像

镜像是运行容器的前提，我们可以使用 `docker pull[IMAGE_NAME]:[TAG]`命令来下载镜像，其中 `IMAGE_NAME` 表示的是镜像的名称，而 `TAG` 是镜像的标签，也就是说我们需要通过 “**镜像 + 标签**” 的方式来下载镜像。

**注意：** 您也可以不显式地指定 TAG, 它会默认下载 latest 标签，也就是下载仓库中最新版本的镜像。这里并不推荐您下载 latest 标签，因为该镜像的内容会跟踪镜像的最新版本，并随之变化，所以它是不稳定的。在生产环境中，可能会出现莫名其妙的 bug, 推荐您最好还是显示的指定具体的 TAG。

举个例子，如我们想要下载一个 Mysql 5.7 镜像，可以通过命令来下载：

```
docker pull mysql:5.7
```

会看到控制台输出内容如下：

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472704673.png)

**注意：** 由于官方 DockerHub 仓库服务器在国外，下载速度较慢，所以我将仓库的地址更改成了国内的 `docker.io` 的镜像仓库，所以在上图中，镜像前面会有 `docker.io` 出现。

当有 **Downloaded** 字符串输出的时候，说明下载成功了！！

### 1.2 验证

让我们来验证一下，本地是否存在 Mysql5.7 的镜像，运行命令：

```
docker images
```

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472703785.png)

可以看到本地的确存在该镜像，确实是下载成功了！

### 1.3 下载镜像相关细节

再说说上面下载镜像的过程：

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472703814.png)

通过下载过程，可以看到，一个镜像一般是由多个层（ `layer`） 组成，类似 `9fc222b64b0a`这样的串表示层的唯一 ID（实际上完整的 ID 包括了 256 个 bit, 64 个十六进制字符组成）。

**您可能会想，如果多个不同的镜像中，同时包含了同一个层（ layer）,这样重复下载，岂不是导致了存储空间的浪费么?**

实际上，Docker 并不会这么傻会去下载重复的层（ `layer`）,Docker 在下载之前，会去检测本地是否会有同样 ID 的层，如果本地已经存在了，就直接使用本地的就好了。

**另一个问题，不同仓库中，可能也会存在镜像重名的情况发生, 这种情况咋办？**

严格意义上，我们在使用 `docker pull` 命令时，还需要在镜像前面指定仓库地址( `Registry`), 如果不指定，则 Docker 会使用您默认配置的仓库地址。例如上面，由于我配置的是国内 `docker.io` 的仓库地址，我在 `pull` 的时候，docker 会默认为我加上 `docker.io/library` 的前缀。

如：当我执行 `docker pull mysql:5.7` 命令时，实际上相当于 `docker pull docker.io/mysql:5.7`，如果您未自定义配置仓库，则默认在下载的时候，会在镜像前面加上 DockerHub 的地址。

Docker 通过前缀地址的不同，来保证不同仓库中，重名镜像的唯一性。

### 1.4 PULL 子命令

命令行中输入：

```
docker pull --help
```

会得到如下信息：

```
root@ubuntuweb:/# docker pull --help
Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]
Pull an image or a repository from a registry
Options:  -a, --all-tags                Download all tagged images in the repository      --disable-content-trust   Skip image verification (default true)      --help                    Print usage
```

我们可以看到主要支持的子命令有：

1. `-a,--all-tags=true|false`: 是否获取仓库中所有镜像，默认为否；
2. `--disable-content-trust`: 跳过镜像内容的校验，默认为 true;

## 二 Docker 查看镜像信息

### 2.1 images 命令列出镜像

通过使用如下两个命令，列出本机已有的镜像：

```
docker images
```

或：

```
docker image ls
```

如下图所示：

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472704417.png)

对上述红色标注的字段做一下解释：

- **REPOSITORY**: 来自于哪个仓库；
- **TAG**: 镜像的标签信息，比如 5.7、latest 表示不同的版本信息；
- **IMAGE ID**: 镜像的 ID, 如果您看到两个 ID 完全相同，那么实际上，它们指向的是同一个镜像，只是标签名称不同罢了；
- **CREATED**: 镜像最后的更新时间；
- **SIZE**: 镜像的大小，优秀的镜像一般体积都比较小，这也是我更倾向于使用轻量级的 alpine 版本的原因；

> 注意：图中的镜像大小信息只是逻辑上的大小信息，因为一个镜像是由多个镜像层（ `layer`）组成的，而相同的镜像层本地只会存储一份，所以，真实情况下，占用的物理存储空间大小，可能会小于逻辑大小。

### 2.2 使用 tag 命令为镜像添加标签

通常情况下，为了方便在后续工作中，快速地找到某个镜像，我们可以使用 `docker tag` 命令，为本地镜像添加一个新的标签。为 `mysql` 镜像，添加新的镜像标签 `allen_mysql:5.7`。然后使用 `docker images` 命令，查看本地镜像。如下图所示：

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472703839.png)


可以看到，本地多了一个 `qymua_mysql:5.7` 的镜像。细心的你一定还会发现， `qymua_mysql:5.7` 和 `mysql:5.7` 的镜像 ID 是一模一样的，说明它们是同一个镜像，只是别名不同而已。

`docker tag` 命令功能更像是, 为指定镜像添加快捷方式一样。

### 2.3 使用 inspect 命令查看镜像详细信息

通过 `docker inspect` 命令，我们可以获取镜像的详细信息，其中，包括创建者，各层的数字摘要等。

```
docker inspect mysql:5.7
```

`docker inspect` 返回的是 `JSON` 格式的信息，如果您想获取其中指定的一项内容，可以通过 `-f` 来指定，如获取镜像大小：

```
docker inspect -f {{".Size"}} mysql:5.7
```

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472704434.png)

### 2.4 使用 history 命令查看镜像历史

前面的小节中，我们知道了，一个镜像是由多个层（layer）组成的，那么，我们要如何知道各个层的具体内容呢？

通过 `docker history` 命令，可以列出各个层（layer）的创建信息，如我们查看 `mysql:5.7` 的各层信息：

```
docker history mysql:5.7
```

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472704061.png)

可以看到，上面过长的信息，为了方便展示，后面都省略了，如果您想要看具体信息，可以通过添加 `--no-trunc` 选项，如下面命令：

```
docker history --no-trunc mysql:5.7
```

## 三 Docker 搜索镜像

### 3.1 search 命令

您可以通过下面命令进行搜索：

```
docker search [option] keyword
```

比如，您想搜索仓库中 `mysql` 相关的镜像，可以输入如下命令：

```
docker search mysql
```

### 3.2 search 子命令

命令行输入 `docker search--help`, 输出如下：

```
Usage:  docker search [OPTIONS] TERM
Search the Docker Hub for images
Options:  -f, --filter filter   Filter output based on conditions provided      --help            Print usage      --limit int       Max number of search results (default 25)      --no-index        Don't truncate output      --no-trunc        Don't truncate output
```

可以看到 `search` 支持的子命令有：

- `-f,--filter filter`: 过滤输出的内容；
- `--limitint`：指定搜索内容展示个数;
- `--no-index`: 不截断输出内容；
- `--no-trunc`：不截断输出内容；

举个列子，比如我们想搜索官方提供的 mysql 镜像，命令如下：

```
docker search --filter=is-offical=true mysql
```

再比如，我们想搜索 Stars 数超过 100 的 mysql 镜像：

```
docker search --filter=stars=100 mysql
```

## 四 Docker 删除镜像

### 4.1 通过标签删除镜像

通过如下两个都可以删除镜像：

```
docker rmi [image]
```

或者：

```
docker image rm [image]
```

支持的子命令如下：

- `-f,-force`: 强制删除镜像，即便有容器引用该镜像；
- `-no-prune`: 不要删除未带标签的父镜像；

例如，我们想删除上章节创建的 `qymua_mysql:5.7` 镜像，命令如下：

```shell
docker rmi qymua_mysql:5.7
```

**实际上，当同一个镜像拥有多个标签时，执行 `docker rmi` 命令，只是会删除了该镜像众多标签中，您指定的标签而已，并不会影响原始的那个镜像文件。**

那么，如果某个镜像不存在多个标签，当且仅当只有一个标签时，执行删除命令时，您就要小心了，这会彻底删除镜像。

### 4.2 通过 ID 删除镜像

除了通过标签名称来删除镜像，我们还可以通过制定镜像 ID, 来删除镜像，如：

```
docker rmi ee7cbd482336
```

一旦制定了通过 ID 来删除镜像，它会先尝试删除所有指向该镜像的标签，然后在删除镜像本身。

### 4.3 删除镜像的限制

删除镜像很简单，但也不是我们何时何地都能删除的，它存在一些限制条件。

当通过该镜像创建的容器未被销毁时，镜像是无法被删除的。

除非通过添加 `-f` 子命令，也就是强制删除，才能移除掉该镜像

```
docker rmi -f mysql:5.7
```

但是，一般不推荐这样暴力的做法，正确的做法应该是：

1. 先删除引用这个镜像的容器；
2. 再删除这个镜像；

也就是，根据引用该镜像的容器 ID ( `9d59e2278553`), 执行删除命令：

```
docker rm 9d59e2278553
```

然后，再执行删除镜像的命令：

```
docker rmi 5cb3aa00f899
```

### 4.4 清理镜像

我们在使用 Docker 一段时间后，系统一般都会残存一些临时的、没有被使用的镜像文件，可以通过以下命令进行清理：

```
docker image prune
```

它支持的子命令有：

- `-a,--all`: 删除所有没有用的镜像，而不仅仅是临时文件；
- `-f,--force`：强制删除镜像文件，无需弹出提示确认；

## 五 Docker 创建镜像

Docker 创建镜像主要有三种：

1. 基于已有的镜像创建；
2. 基于 Dockerfile 来创建；
3. 基于本地模板来导入；

我们将主要介绍常用的 1，2 两种。

### 5.1 基于已有的镜像创建

通过如下命令来创建：

```
docker container commit
```

支持的子命令如下：

- `-a,--author`="": 作者信息；
- `-c,--change`=[]: 可以在提交的时候执行 Dockerfile 指令，如 CMD、ENTRYPOINT、ENV、EXPOSE、LABEL、ONBUILD、USER、VOLUME、WORIR 等；
- `-m,--message`="": 提交信息；
- `-p,--pause`=true: 提交时，暂停容器运行。

接下来，基于本地已有的 Ubuntu 镜像，创建一个新的镜像：

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472704307.png)

首先，让我将它运行起来，并在其中创建一个 test.txt 文件：

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472704499.png)

命令如下：

```
root@ubuntuweb:/# docker run -it ubuntu:latest /bin/bash
root@437b2645a468:/# touch test.txt
root@437b2645a468:/# exit
```

创建完 test.txt 文件后，需要记住标注的容器 ID: `a0a0c8cfec3a`, 用它来提交一个新的镜像(**PS: 你也可以通过名称来提交镜像，这里只演示通过 ID 的方式**)。

执行命令：

```
docker container commit -m "Added test.txt file" -a "QYMUA" 437b2645a468 test:0.1
```

提交成功后，会返回新创建的镜像 ID 信息，如下图所示：

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472703606.png)

再次查看本地镜像信息，可以看到新创建的 `test:0.1` 镜像了：

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472703556.png)

### 5.2 基于 Dockerfile 创建

通过 Dockerfile 的方式来创建镜像，是最常见的一种方式了，也是比较推荐的方式。Dockerfile 是一个文本指令文件，它描述了是如何基于一个父镜像，来创建一个新镜像的过程。

下面让我们来编写一个简单的 Dockerfile 文件，它描述了基于 Ubuntu 父镜像，安装 Python3 环境的镜像：

```
FROM ubuntu:latest
LABEL version="1.0" maintainer="QYMUA <Qymua@github>"
RUN apt-get update && \    apt-get install -y python3 && \    apt-get clean && \    rm -rf /var/lib/apt/lists/*
```

创建完成后，通过这个 Dockerfile 文件，来构建新的镜像，执行命令：

```
docker image build -t python:3 .
```

**注意：** 命令的最后有个点，如果不加的话，会构建不成功 

## 六 Docker 导出&加载镜像

通常我们会有下面这种需求，需要将镜像分享给别人，这个时候，我们可以将镜像导出成 tar 包，别人直接通过加载这个 tar 包，快速地将镜像引入到本地镜像库。

要想使用这两个功能，主要是通过如下两个命令：

1. `docker save`
2. `docker load`

### 6.1 导出镜像

例如，我们想要将 test:0.1 镜像导出来，执行命令：

```
docker save -o test_0.1.tar test:0.1
```

执行成功后，查看当前目录：

![](https://cdn.jsdelivr.net/gh/qoo3/imgur@master/bookmaker/1589472703997.png)

可以看到 `test_0.1.tar` 镜像文件已经生成

### 6.2 加载镜像

别人拿到了这个 `tar` 包后，要如何导入到本地的镜像库呢？

通过执行如下命令：

```
docker load -i test_0.1.tar
```

或者：

```
docker load < test_0.1.tar
```

导入成功后，查看本地镜像信息，你就可以获得别人分享的镜像了！怎么样，是不是很方便呢！

## 七 Docker 上传镜像

以上传到 Docker Hub 上为示例，演示 Docker 如何上传镜像。

### 7.1 获取 Docker ID

想要上传镜像到 Docker Hub 上，首先，我们需要注册 Docker Hub 账号。打开 Docker Hub 网址 https://hub.docker.com，开始注册：

使用方法和github大致相同，创建仓库即可

### 7.2 上传镜像

进入命令行，**用我们刚刚获取的 Docker ID 以及密码登录**，执行命令：

```
docker login
```

命令行登录 Docker ID

登录成功后，我们开始准备上传本地的 镜像：

首先，我们对其打一个新的标签，**前缀与我们新创建的 Docker ID 、仓库名保持一致**:

```
docker tag test:0.1 dockerID/test:0.1
```

查看本地信息，可以看到，标签打成功了。执行命令：

```
docker push dockerID/test:0.1
```



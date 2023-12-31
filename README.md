# github-proxy

代理服务。

这只是后端服务，前端页面需要自己完成并通过静态文件导入。

## 编译后端服务

```bash
go build -ldflags="-s -v" -o main main.go && upx -9 main
```

- -s: 忽略符号表和调试信息
- -w: 忽略DWARFv3调试信息，使用该选项后将无法使用gdb进行调试

用 upx 压缩能大幅缩小可执行文件体积，如果对程序的体积没有要求，可以不执行此步，因为使用 upx 压缩后在执行时会有一个用时很短的解压过程。

如果你是用 Windows 作为开发平台，应逐行执行以下命令进行交叉编译：

```powershell
$Env:CGO_ENABLED=0
$Env:GOOS='linux'
$Env:GOARCH='amd64'
go build -ldflags="-s -v" -o main main.go && upx -9 main
```

### upx

下载对应系统的最新 [release](https://github.com/upx/upx/releases/latest) ，放到 PATH 中即可。

### 配置前端页面

如果你不会自己写前端页面可使用我打包的页面，但里面有一张捐赠图片，你可以选择删除或替换。

已打包的前端页面：

https://pan.baidu.com/s/14iEOGT9xSAR1EUXTpoO01g 提取码: xvme

将前端文件解压后放到任意目录(最好叫`static`)中，然后修改`main.go`中的`"../caddy/static"`为前端文件目录。

### 测试运行

在后端根目录中执行：

```bash
./main
```

使用了 3000 端口，访问`http://localhost:3000`即可访问首页，输入某个 github 的文件链接即可测试下载功能。

### 部署

上线建议使用 caddy，可以基于 docker 容器，也可以直接安装在物理机中。

需要`static`目录和编译好的`main`文件复制到 caddy 环境中，使用 caddy 的反向代理即可。

caddy 配置文件：

```caddy
<域名> # 如 example.com

reverse_proxy localhost:3000
```

## 相关项目

- [gh-proxy](https://github.com/hunshcn/gh-proxy)

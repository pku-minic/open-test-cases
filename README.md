# 北大编译实习课程公开测试用例

该 repo 内存储了北京大学编译实习课程所提供的公开的编译器功能/性能测试用例.

## clone 当前 repo

你可以执行以下命令:

```
$ git clone --recursive https://github.com/pku-minic/open-test-cases.git
```

## 目录结构

* `sysy` 目录: 大赛公开的 SysY 功能/性能测试.
* `eeyore` 目录: 公开的 Eeyore 功能/性能测试, 输入/输出与 SysY 测试用例相同.
* `tigger` 目录: 公开的 Tigger 功能/性能测试, 输入/输出与 SysY 测试用例相同.
* `risc-v` 目录: 可构建 RISC-V 开发环境的 `Dockerfile`.

## 相关工具

> 推荐你使用 [oj-docker](https://github.com/pku-minic/oj-docker) 中的 Dockerfile 来构建包含全套 OJ 工具链的 Docker 镜像.

若你需要测试你的编译器生成的 RISC-V 汇编是否正确, 你可以使用我们提供的 [`Dockerfile`](risc-v/Dockerfile), 构建一个可编译/运行 32-bit RISC-V 程序的 Docker 镜像. 在此之前, 建议你参考并学习 Docker 提供的官方文档, 安装 `docker`, 然后你可以执行:

```sh
cd risc-v
docker build -t riscv-dev-env .
```

来构建一个 Docker 镜像. 此后, 你可以据此镜像运行一个 Docker 容器, 并将宿主机内的目录挂载到 Docker 中.

假设存放你的编译器生成的 RISC-V 汇编的目录位于宿主机的 `~/asm` 目录, 运行如下命令可将其挂载到容器的 `/root/asm` 目录中:

```sh
docker run -it --rm -v ~/asm:/root/asm riscv-dev-env
```

假设编译器生成的文件为 `asm/output.S`, 你可以在 Docker 容器中执行如下命令来调用 RISC-V 工具链和 QEMU 汇编并运行该程序:

```sh
riscv32-unknown-linux-gnu-gcc asm/output.S -o output -L/root -lsysy -static
qemu-riscv32-static output
```

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

你可以使用 [MiniVM](https://github.com/pku-minic/open-test-cases) 来测试编译器生成的 Eeyore/Tigger 是否正确.

若你需要测试你的编译器生成的 RISC-V 汇编是否正确, 你可以使用我们提供的 [`Dockerfile`](risc-v/Dockerfile), 构建一个可编译/运行 32-bit RISC-V 程序的 Docker 镜像. 在此之前, 建议你参考并学习 Docker 提供的官方文档, 安装 `docker`, 然后你可以执行:

```
$ cd risc-v
$ docker build -t riscv-dev-env .
```

来构建一个 Docker 镜像. 此后, 你可以将宿主机内的目录设为与 Docker 共享, 然后执行:

```
$ docker run -it --rm riscv-dev-env
```

来运行构建完毕的镜像. 假设编译器生成的文件为 `output.S`, 你可以在 Docker 容器中执行如下命令来调用 RISC-V 工具链和 QEMU 汇编并运行该程序:

```
$ riscv32-unknown-linux-gnu-gcc output.S -o output -L/root -lsysy -static
$ qemu-riscv32-static output
```

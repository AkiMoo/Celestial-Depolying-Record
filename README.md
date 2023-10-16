# Celestial-Depolying-Record

## （因为硬件部署困难，没办法在虚拟机上再运用microVM技术，转为将代码核心部分构建容器再组网）
## 一些前置知识学习记录
### Dockerfile 的常用指令
（RUN,COPY,ADD）会创建新的层
FROM 指令用于初始化一个新的构建阶段，并为后续指令指定一个基础镜像。
如果基础镜像有多个平台版本，可以通过 --platform 来指定要使用哪个版本，如，linux/amd64。
可以通过 AS <name> 为当前构建阶段指定一个名字，后续可通过 COPY --from=<name> 来引用此阶段所构建的镜像。

ENV 指令用于设置环境变量，其设置的环境变量可以被后续指令，且会持久地存储，容器运行时依旧存在；可以被 docker run 命令覆盖。

WORKDIR 指令用于为后续的 RUN, CMD, ENTRYPOINT, ADD, COPY 指令指定工作路径。如果后面的 WORKDIR 指令的参数是一个相对路径，则它是相对于上一个 WORKDIR 指令指定的工作路径的。

RUN 指令会在当前镜像之上执行给定的命令，并将执行结果作为一个新的层。
![RUN command](https://github.com/AkiMoo/Celestial-Depolying-Record/assets/32764968/7be5beaf-c283-44c7-9519-31847c1790ed)
第一种形式是在 shell 中执行命令，也就是说基础镜像需要包含指定的 shell 才行；
第二种形式是 exec 形式，不会调用 shell（也就不会执行环境变量替换），可以执行任意可执行文件，其参数会被解析成 JSON 数组，所以需要使用双引号！
![RUN command2](https://github.com/AkiMoo/Celestial-Depolying-Record/assets/32764968/90a027f8-b417-4723-a214-c00fdce999ad)

ADD 指令用于拷贝本地文件、目录、远程文件至镜像中。

COPY 指令和 ADD 指令类似，但不支持从远程拷贝文件，此外它也不会自动解包 tar 文件。

VOLUME 指令用于将容器中指定的目录挂载为匿名卷，可以通过 docker inspect 命令来查看其挂载到了主机哪个位置。

CMD 指令有3种形式，前两种和 RUN 指令类似，但不同于 RUN，CMD 指令指定的命令是在容器运行时执行的（和 ENTRYPOINT 指令功能相同），而不是构建容器时；第三种形式的作用是给 ENTRYPOINT 指令提供默认参数（可以被 docker run 命令指定的参数给覆盖掉）。

ENTRYPOINT 指令用于指定容器启动（执行 docker run <image>）时的默认动作。 类似于 RUN 指令，ENTRYPOINT 指令也有 shell 和 exec 两种形式。
![ENTRYPOINT](https://github.com/AkiMoo/Celestial-Depolying-Record/assets/32764968/740f046b-1a30-4d55-93b6-f7592671b5b3)

**多阶段构建**
多阶段构建可以有效地减小镜像的大小。它是通过为每个构建阶段提供一个名字，并结合 COPY --from=<build-stage-name> 来实现的，即，从其它构建阶段构建的镜像中拷贝需要的内容至当前镜像，如此可以大大减小最终镜像的体积。

#### requirements.txt 里面指定需要的库和版本依赖
#### go.mod 是Go语言的官方包管理工具，用于解决之前没有地方记录依赖包具体版本的问题，方便依赖包的管理。Go.mod其实就是一个Modules，关于Modules的官方定义为：Modules是相关Go包的集合，其中包含了它们的依赖关系和版本信息，它们被组织在一起，并以一种允许Go语言编译器、测试工具和其他工具访问它们的方式进行管理。在Go语言中，使用Go.mod文件来定义项目的依赖关系，并且在编译项目时，Go会自动下载和安装所需要的依赖包及其版本信息。
#### 为了确保一致性构建，Go引入了go.mod文件来标记每个依赖包的版本，在构建过程中go命令会下载go.mod中的依赖包，下载的依赖包会缓存在本地，以便下次构建。 考虑到下载的依赖包有可能是被黑客恶意篡改的，以及缓存在本地的依赖包也有被篡改的可能，单单一个go.mod文件并不能保证一致性构建。

On Wmware Ubuntu 18.04
# First Fix "Virtualized Intel VT-x/EPT is not supported on this platform. Continue without virtualized Intel VT-x/EPT"
1, Windows Task Manager Check *Virtualized*
2, Turn Windows Features on or off 
Hyper-V

Virtual Machine Platform

Windows Hypervisor Platform

Windows Subsystem for Linux
![image](https://github.com/AkiMoo/Celestial-Depolying-Record/assets/32764968/54d47a71-f704-4d53-acae-6fa957cd4564)

3, Windows Security => Device Security => Core isolation,
Memory integrity

Local Security Authority protection
![image](https://github.com/AkiMoo/Celestial-Depolying-Record/assets/32764968/86f6442d-f6cb-4385-bbd4-4cdba96ceb3d)

Finally,
![image](https://github.com/AkiMoo/Celestial-Depolying-Record/assets/32764968/466ce5dd-e996-44b1-a797-933ec65d313c)

# Second fix "cd ~/celestial // docker build -f compile.Dockerfile -t celestial-make ."

socket need root authorized, so it need to switch to root.

and another problem is go & golang
1, apt install golang-go
answer in here: https://blog.csdn.net/m0_56203480/article/details/127637912
2, https://www.cnblogs.com/zhangmingcheng/p/15870370.html cant fix

# systemstap container

### 安装

1. 安装systemstap
2. 安装需要的kenerl依赖包

### 运行容器

```shell
docker run --network host --cap-add=SYS_MODULE  --security-opt seccomp=unconfined -v /home/deeproute/workspace/ceph:/ceph -v /sys/kernel/debug:/sys/kernel/debug -v /usr/src/kernels:/usr/src/kernels -v /usr/lib/modules/:/usr/lib/modules/ -v /usr/lib/debug:/usr/lib/debug  -ti zhucan/centos:stream-env bash
```

### 参考资料

- https://sourceware.org/systemtap/SystemTap_Beginners_Guide.pdf
- https://imroc.cc/kubernetes/troubleshooting/skill/kernel/use-systemtap-to-locate-problems.html
- https://github.com/container-images/systemtap/blob/master/Dockerfile
- https://buildlogs.centos.org/c7.2003.00.x86_64/kernel/20200331233310/3.10.0-1127.el7.x86_64/
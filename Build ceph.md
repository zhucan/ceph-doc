# Build ceph  in the container

### 编译环境

- ceph branch: v16.2.12

- compile image: quay.io/centos/centos:stream

### 编译二进制

```shell
$ cd /home/xxx/workspace # 此目录用户指定，用于下载ceph的代码
$ git clone git@github.com:ceph/ceph
$ git checkout -b v16.2.12 v16.2.12
$ docker run -v /home/xxx/workspace/ceph:/ceph -ti quay.io/centos/centos:stream bash
$ cd /ceph
$ 修改./install-deps.sh脚本， 替换'pip >= 7.0'为'pip >= 21.0'
$ dnf install git
$ ./install-deps.sh
$ ./do_cmake.sh -DCMAKE_BUILD_TYPE=RelWithDebInfo
$ cd ./build && make -j8
```

### 编译制作rpm包

```shell
$ cd /home/xxx/workspace # 此目录用户指定，用于下载ceph的代码
$ git clone git@github.com:ceph/ceph
$ git checkout -b v16.2.12 v16.2.12
$ docker run -v /home/xxx/workspace/ceph:/ceph -ti quay.io/centos/centos:stream bash
$ cd /ceph
$ 修改./install-deps.sh脚本， 替换'pip >= 7.0'为'pip >= 21.0'
$ dnf install git
$ ./install-deps.sh
$ ./make-srpm.sh #执行完成后会在当前目录下生成一个 ceph-16.2.12-0.g5a2d516ce4b.el8.src.rpm的src rpm包
$ ./dnf install rpm-build rpmdevtools
$ rpmdev-setuptree
$ rpm -i ceph-16.2.12-0.g5a2d516ce4b.el8.src.rpm
$ rpm -ba ~/rpmbuild/SPECS/ceph.spec

等待rpm包制作完成，如下：
[root@319a80f2c3c9 ceph]# tree ~/rpmbuild/RPMS/
/root/rpmbuild/RPMS/
├── noarch
│   ├── ceph-grafana-dashboards-16.2.12-0.g5a2d516ce4b.el8.noarch.rpm
│   ├── ceph-mgr-cephadm-16.2.12-0.g5a2d516ce4b.el8.noarch.rpm
│   ├── ceph-mgr-dashboard-16.2.12-0.g5a2d516ce4b.el8.noarch.rpm
│   ├── ceph-mgr-diskprediction-local-16.2.12-0.g5a2d516ce4b.el8.noarch.rpm
│   ├── ceph-mgr-k8sevents-16.2.12-0.g5a2d516ce4b.el8.noarch.rpm
│   ├── ceph-mgr-modules-core-16.2.12-0.g5a2d516ce4b.el8.noarch.rpm
│   ├── ceph-mgr-rook-16.2.12-0.g5a2d516ce4b.el8.noarch.rpm
│   ├── ceph-prometheus-alerts-16.2.12-0.g5a2d516ce4b.el8.noarch.rpm
│   ├── cephadm-16.2.12-0.g5a2d516ce4b.el8.noarch.rpm
│   └── cephfs-top-16.2.12-0.g5a2d516ce4b.el8.noarch.rpm
└── x86_64
    ├── ceph-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-base-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-base-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-common-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-common-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-debugsource-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-fuse-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-fuse-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-immutable-object-cache-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-immutable-object-cache-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-mds-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-mds-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-mgr-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-mgr-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-mon-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-mon-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-osd-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-osd-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-radosgw-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-radosgw-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-resource-agents-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-selinux-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-test-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── ceph-test-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── cephfs-mirror-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── cephfs-mirror-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── libcephfs-devel-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── libcephfs2-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── libcephfs2-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── libcephsqlite-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── libcephsqlite-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── libcephsqlite-devel-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── librados-devel-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── librados-devel-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── librados2-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── librados2-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── libradospp-devel-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── libradosstriper-devel-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── libradosstriper1-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── libradosstriper1-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── librbd-devel-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── librbd1-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── librbd1-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── librgw-devel-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── librgw2-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── librgw2-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── python3-ceph-argparse-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── python3-ceph-common-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── python3-cephfs-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── python3-cephfs-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── python3-rados-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── python3-rados-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── python3-rbd-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── python3-rbd-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── python3-rgw-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── python3-rgw-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── rados-objclass-devel-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── rbd-fuse-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── rbd-fuse-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── rbd-mirror-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── rbd-mirror-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    ├── rbd-nbd-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm
    └── rbd-nbd-debuginfo-16.2.12-0.g5a2d516ce4b.el8.x86_64.rpm

2 directories, 73 files
```

生成rpm时，遇到内存不足问题，扩容内存，"dmesg  -T"查看日志信息

```
[Wed Apr 26 15:46:13 2023] Out of memory: Kill process 99967 (cc1plus) score 92 or sacrifice child
[Wed Apr 26 15:46:13 2023] Killed process 99967 (cc1plus), UID 0, total-vm:3118864kB, anon-rss:3032800kB, file-rss:0kB, shmem-rss:0kB
```

### 编译制作容器镜像

```shell
$ git clone git@github.com:ceph/ceph-container.git
$ cd ./ceph-container
$ make FLAVORS=pacific,centos,8 build
```

- 参考文档链接：https://github.com/ceph/ceph-container

- 遇到问题：https://github.com/ceph/ceph-container/issues/2113

- 备注: 该镜像是基于ceph pacific版本的rpm包（[Index of /](http://download.ceph.com/)）编译完成，官方文档有备注说明能支持基于分支编译，本人还未测试过
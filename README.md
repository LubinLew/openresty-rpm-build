# openresty-rpm-build
build openresty rpm package on CentOS7
> https://github.com/openresty/openresty-packaging

```bash
#!/usr/bin/bash

## install tools
yum install -y git gcc rpm-build rpmdevtools ccache systemtap-sdt-devel

## install openresty-devel packages
wget https://openresty.org/package/centos/openresty.repo
mv openresty.repo /etc/yum.repos.d/
yum install -y openresty-zlib openresty-zlib-devel
yum install -y openresty-pcre openresty-pcre-devel
yum install -y openresty-openssl111 openresty-openssl111-devel

## downlaod openresty spec
git clone https://github.com/openresty/openresty-packaging.git

## config rpmbuild
mkdir -p ~/rpmbuild/{BUILD,RPMS,SOURCES,SPECS,SRPMS}
cp openresty-packaging/rpm/SOURCES/*    ~/rpmbuild/SOURCES/
cp openresty-packaging/rpm/SPECS/openresty.spec ~/rpmbuild/SPECS/

## build the rpm
cd ~/rpmbuild/SPECS/
spectool -g -R openresty.spec
rpmbuild -ba openresty.spec
```

## rpm packages

```bash
[root@centos x86_64]# rpm -ql openresty-zlib
/usr/local/openresty/zlib/lib/libz.so
/usr/local/openresty/zlib/lib/libz.so.1
/usr/local/openresty/zlib/lib/libz.so.1.2.11

[root@centos x86_64]# rpm -ql openresty-pcre
/usr/local/openresty/pcre/lib/libpcre.so
/usr/local/openresty/pcre/lib/libpcre.so.1
/usr/local/openresty/pcre/lib/libpcre.so.1.2.12

[root@centos x86_64]# rpm -ql openresty-openssl111
/usr/local/openresty/openssl111/bin/openssl
/usr/local/openresty/openssl111/lib/engines-1.1/capi.so
/usr/local/openresty/openssl111/lib/engines-1.1/padlock.so
/usr/local/openresty/openssl111/lib/libcrypto.so
/usr/local/openresty/openssl111/lib/libcrypto.so.1.1
/usr/local/openresty/openssl111/lib/libssl.so
/usr/local/openresty/openssl111/lib/libssl.so.1.1
```

## nginx parameters

```bash
--with-cc-opt='-O2 -DNGX_LUA_ABORT_AT_PANIC -I/usr/local/openresty/zlib/include -I/usr/local/openresty/pcre/include -I/usr/local/openresty/openssl111/include'
--with-ld-opt='-Wl,-rpath,/usr/local/openresty/luajit/lib -L/usr/local/openresty/zlib/lib -L/usr/local/openresty/pcre/lib -L/usr/local/openresty/openssl111/lib -Wl,-rpath,/usr/local/openresty/zlib/lib:/usr/local/openresty/pcre/lib:/usr/local/openresty/openssl111/lib'
```


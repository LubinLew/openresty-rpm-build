# openresty-rpm-build
build openresty rpm package on CentOS7

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

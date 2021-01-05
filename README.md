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
[root@centos x86_64]# openresty -V
nginx version: openresty/1.19.3.1
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
built with OpenSSL 1.1.1i  8 Dec 2020
TLS SNI support enabled
configure arguments: --prefix=/usr/local/openresty/nginx --with-cc-opt='-O2 -DNGX_LUA_ABORT_AT_PANIC `-I/usr/local/openresty/zlib/include` `-I/usr/local/openresty/pcre/include` `-I/usr/local/openresty/openssl111/include`' --add-module=../ngx_devel_kit-0.3.1 --add-module=../echo-nginx-module-0.62 --add-module=../xss-nginx-module-0.06 --add-module=../ngx_coolkit-0.2 --add-module=../set-misc-nginx-module-0.32 --add-module=../form-input-nginx-module-0.12 --add-module=../encrypted-session-nginx-module-0.08 --add-module=../srcache-nginx-module-0.32 --add-module=../ngx_lua-0.10.19 --add-module=../ngx_lua_upstream-0.07 --add-module=../headers-more-nginx-module-0.33 --add-module=../array-var-nginx-module-0.05 --add-module=../memc-nginx-module-0.19 --add-module=../redis2-nginx-module-0.15 --add-module=../redis-nginx-module-0.3.7 --add-module=../ngx_stream_lua-0.0.9 --with-ld-opt='`-Wl,-rpath,/usr/local/openresty/luajit/lib -L/usr/local/openresty/zlib/lib -L/usr/local/openresty/pcre/lib -L/usr/local/openresty/openssl111/lib -Wl,-rpath,/usr/local/openresty/zlib/lib:/usr/local/openresty/pcre/lib:/usr/local/openresty/openssl111/lib`' --with-cc='ccache gcc -fdiagnostics-color=always' --with-pcre-jit --with-stream --with-stream_ssl_module --with-stream_ssl_preread_module --with-http_v2_module --without-mail_pop3_module --without-mail_imap_module --without-mail_smtp_module --with-http_stub_status_module --with-http_realip_module --with-http_addition_module --with-http_auth_request_module --with-http_secure_link_module --with-http_random_index_module --with-http_gzip_static_module --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-threads --with-compat --with-stream --with-http_ssl_module


```

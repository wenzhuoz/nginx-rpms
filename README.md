# Building ngx_http_geoip2_module for CentOS 8

* CentOS nginx package git: https://git.centos.org/rpms/nginx/
* ngx_http_geoip2_module: https://github.com/leev/ngx_http_geoip2_module

## Build environment
1. Install development tools
    ```
    $ sudo dnf group install "Development Tools" "RPM Development Tools"
    ```

2. libmaxminddb-devel, which is required to build the ngx_http_geoip2_module, is missing from CentOS 8.0. We need to grab the libmaxminddb src rpm and rebuild it.
    ```
    $ rpmdev-setuptree
    $ dnf download --source libmaxminddb
    $ rpmbuild --rebuild libmaxminddb-1.2.0-6.el8.src.rpm
    $ sudo rpm -Uvh ~/rpmbuild/RPMS/x86_64/libmaxminddb-devel-1.2.0-6.el8.x86_64.rpm
    ```

3. CentOS get_sources.sh script
    ```
    $ git clone https://git.centos.org/centos-git-common.git
    $ mkdir -p ~/bin && ln -sf `pwd`/centos-git-common/get_sources.sh ~/bin
    ```

## Rebuilding nginx rpms with the ngx_http_geoip2_module
```
$ sudo dnf builddep nginx
$ git clone -b c8-stream-1.14 https://github.com/wenzhuoz/nginx-rpms.git
$ cd nginx-rpms
$ get_sources.sh
$ rpmbuild --define "%_topdir `pwd`" -ba SPECS/nginx.spec --with geoip2
```

## See also
* https://wiki.centos.org/Sources

# Building ngx_http_geoip2_module for CentOS 8

* CentOS nginx package git: https://git.centos.org/rpms/nginx/
* ngx_http_geoip2_module: https://github.com/leev/ngx_http_geoip2_module

## Build environment
1. Install development tools
    ```
    $ sudo dnf group install "Development Tools" "RPM Development Tools"
    ```

2. Set up rpmbuild directory
    ```
    $ rpmdev-setuptree
    ```

3. libmaxminddb-devel, which is required to build the ngx_http_geoip2_module, is missing from CentOS 8.0. We need to grab the libmaxminddb src rpm and rebuild it.
    ```
    $ dnf download --source libmaxminddb
    $ rpmbuild --rebuild libmaxminddb-1.2.0-6.el8.src.rpm
    $ sudo rpm -Uvh ~/rpmbuild/RPMS/x86_64/libmaxminddb-devel-1.2.0-6.el8.x86_64.rpm
    ```

## Rebuilding nginx rpm with the ngx_http_geoip2_module
```
$ sudo yum-builddep nginx
$ dnf download --source nginx
$ rpm -ivh nginx-1.14.1-8.module_el8.0.0+5+258f653c.src.rpm
$ git clone -b c8-stream-1.14 https://github.com/wenzhuoz/nginx-rpmbuild.git
$ cd nginx-rpmbuild
$ cp SPECS/nginx.spec ~/rpmbuild/SPECS/
$ cp SOURCES/* ~/rpmbuild/SOURCES/
$ cd ~/rpmbuild/SPECS/
$ rpmbuild -ba nginx.spec --with geoip2
```

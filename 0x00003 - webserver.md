# nginx foi o web server utilizado, não é de costume e os modulos são um saco para adicionar.

-/usr/local/nginx/conf
-/etc/nginx/nginx.conf
-/usr/local/etc/nginx


## Instalando

```
# apt install -y nginx
# systemctl restart nginx
# systemctl status nginx
```

## Instalação WAF (ModSecurity)

```
# apt-get install bison ca-certificates dh-autoreconf doxygen flex gawk iputils-ping libcurl4-gnutls-dev libexpat1-dev libgeoip-dev liblmdb-dev libpcre3-dev libpcre++-dev libssl-dev libtool libxml2 libxml2-dev libyajl-dev locales lua5.3-dev pkg-config wget zlib1g-dev zlibc

# git clone https://github.com/ssdeep-project/ssdeep
# cd ssdeep/
# ./bootstrap
# ./configure
# make
# make install
# cd ..

# git clone https://github.com/SpiderLabs/ModSecurity
# cd ModSecurity
# git submodule init
# git submodule update
# ./build.sh
# ./configure
# make
# make install
# cd ..

# git clone https://github.com/SpiderLabs/ModSecurity-nginx.git
# nginx -v
nginx version: nginx/1.14.2
# wget http://nginx.org/download/nginx-1.14.2.tar.gz
# tar -zxvf nginx-1.14.2.tar.gz
# cd nginx-1.14.2/
# ./configure --with-compat --add-dynamic-module=../ModSecurity-nginx
# make modules
# cp objs/ngx_http_modsecurity_module.so /usr/share/nginx/modules/
# cp objs/ngx_http_modsecurity_module.so /etc/nginx/modules
# cd ..

# git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git
# cp owasp-modsecurity-crs/crs-setup.conf.example owasp-modsecurity-crs/crs-setup.conf
# cp owasp-modsecurity-crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example owasp-modsecurity-crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
# cp -r owasp-modsecurity-crs/ /usr/local
```

## Configuração WAF (ModSecurity)

```
# cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bkp
# vim /etc/nginx/nginx.conf
load_module modules/ngx_http_modsecurity_module.so;

# mkdir -p /etc/nginx/modsec
# cp ~/Downloads/ModSecurity/unicode.mapping /etc/nginx/modsec
# cp ~/Downloads/ModSecurity/modsecurity.conf-recommended /etc/nginx/modsec/modsecurity.conf
# vim /etc/nginx/modsec/modsecurity.conf
SecRuleEngine On
SecAuditLog /var/log/nginx/modsec_audit.log

# vim /etc/nginx/modsec/main.conf
Include "/etc/nginx/modsec/modsecurity.conf"
Include "/usr/local/owasp-modsecurity-crs/crs-setup.conf"
Include "/usr/local/owasp-modsecurity-crs/rules/*.conf"

# vim /etc/nginx/sites-available/default
server {
        listen 80 default_server;
        listen [::]:80 default_server;
	modsecurity on;
	modsecurity_rules_file /etc/nginx/modsec/main.conf;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
        server_name _;
        location / {
                try_files $uri $uri/ =404;
        }
}
```

References:
https://nginx.org/en/docs/
https://docs.nginx.com/nginx-waf/
https://github.com/SpiderLabs/ModSecurity-nginx
https://www.howtoforge.com/tutorial/nginx-with-libmodsecurity-and-owasp-modsecurity-core-rule-set-on-ubuntu-1604/
https://techexpert.tips/pt-br/nginx-pt-br/nginx-instalar-modsecurity/
https://mkyong.com/nginx/nginx-modsecurity-and-owasp-crs/

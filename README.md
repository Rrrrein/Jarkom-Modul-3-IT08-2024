# Jarkom-Modul-3-IT08-2024

| Nama          | NRP          |
| ------------- | ------------ |
| Irfan Qobus Salim | 5027221058 |
| Aisha Ayya Ratiandari | 5027231056 |

## Topologi 2
![image](https://github.com/user-attachments/assets/5c666d1f-6e19-4458-9d88-44dc505a71dc)

## Config

---

Config setiap node

### Paradis (DHCP Relay)

```markdown
auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.237.0.0/16
up echo nameserver 192.168.122.1 > /etc/resolv.conf

auto eth1
iface eth1 inet static
	address 192.237.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.237.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.237.3.1
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.237.4.1
	netmask 255.255.255.0
```

### Tybur (DHCP Server)

```markdown
auto eth0
iface eth0 inet static
address 192.237.4.2
netmask 255.255.255.0
gateway 192.237.4.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Fritz (DNS Server)

```markdown
auto eth0
iface eth0 inet static
address 192.237.4.3
netmask 255.255.255.0
gateway 192.237.4.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Warhammer (database)

```markdown
auto eth0
iface eth0 inet static
address 192.237.3.2
netmask 255.255.255.0
gateway 192.237.3.1
up echo nameserver 192.237.4.3 > /etc/resolv.conf
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Beast (LoadBalancer Laravel)

```markdown
auto eth0
iface eth0 inet static
address 192.237.3.4
netmask 255.255.255.0
gateway 192.237.3.1
up echo nameserver 192.237.4.3 > /etc/resolv.conf
up echo nameserver 192.168.122.1 > /etc/resolv.conf

```

### Colossal (LoadBalancer PHP)

```markdown
auto eth0
iface eth0 inet static
address 192.237.3.3
netmask 255.255.255.0
gateway 192.237.3.1
up echo nameserver 192.237.4.3 > /etc/resolv.conf
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Annie (Laravel Worker)

```markdown
auto eth0
iface eth0 inet static
address 192.237.1.2
netmask 255.255.255.0
gateway 192.237.1.1
up echo nameserver 192.237.4.3 > /etc/resolv.conf
up echo nameserver 192.168.122.1 > /etc/resolv.conf

```

### Bertholdt (Laravel Worker)

```markdown
auto eth0
iface eth0 inet static
address 192.237.1.3
netmask 255.255.255.0
gateway 192.237.1.1
up echo nameserver 192.237.4.3 > /etc/resolv.conf
up echo nameserver 192.168.122.1 > /etc/resolv.conf

```

### Reiner (Laravel Worker)

```markdown
auto eth0
iface eth0 inet static
address 192.237.1.4
netmask 255.255.255.0
gateway 192.237.1.1
up echo nameserver 192.237.4.3 > /etc/resolv.conf
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Armin (PHP Worker)

```markdown
auto eth0
iface eth0 inet static
address 192.237.2.2
netmask 255.255.255.0
gateway 192.237.2.1
up echo nameserver 192.237.4.3 > /etc/resolv.conf
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Eren (PHP Worker)

```markdown
auto eth0
iface eth0 inet static
address 192.237.2.3
netmask 255.255.255.0
gateway 192.237.2.1
up echo nameserver 192.237.4.3 > /etc/resolv.conf
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Mikasa (PHP Worker)

```markdown
auto eth0
iface eth0 inet static
address 192.237.2.4
netmask 255.255.255.0
gateway 192.237.2.1
up echo nameserver 192.237.4.3 > /etc/resolv.conf
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Zeke (Client)

```markdown
auto eth0
iface eth0 inet dhcp
```

### Erwin (Client)

```markdown
auto eth0
iface eth0 inet dhcp

```

## No.0

---

Membuat script di Fritz agar domain [marley.it08.com](http://marley.it08.com) ke IP Annie dan [eldia.it08.com](http://eldia.it08.com) ke IP Armin

```markdown
apt-get update
apt-get install bind9 -y

forward="options {
directory \"/var/cache/bind\";
forwarders {
  	   192.168.122.1;
};

allow-query{any;};
listen-on-v6 { any; };
};
"
echo "$forward" > /etc/bind/named.conf.options

echo "zone \"marley.it08.com\" {
	type master;
	file \"/etc/bind/jarkom/marley.it08.com\";
};

zone \"eldia.it08.com\" {
	type master;
	file \"/etc/bind/jarkom/eldia.it08.com\";
};
" > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

riegel="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    marley.it08.com. root.marley.it08.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    marley.it08.com.
@       IN    A    192.237.1.2
"
echo "$riegel" > /etc/bind/jarkom/marley.it08.com

granz="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    eldia.it08.com. root.eldia.it08.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    eldia.it08.com.
@       IN    A    192.237.2.2
"
echo "$granz" > /etc/bind/jarkom/eldia.it08.com

service bind9 restart
```

### No. 1 - 5

---

- Menjalankan service dari isc-dhcp-server di Tybur

```markdown
service isc-dhcp-server start
```

- Menambahkan line berikut pada file /etc/default/isc-dhcp-server di Tybur

```markdown
INTERFACES="eth0"
```

- Membuat script untuk menghubungkan Paradis ke Tybur

```markdown
echo INTERFACESv4="eth0" > /etc/default/isc-dhcp-server

echo 'authoritative;

subnet 192.237.3.0 netmask 255.255.255.0 {
    option routers 192.237.3.0;
    option broadcast-address 192.237.3.255;
}

subnet 192.237.4.0 netmask 255.255.255.0 {
    option routers 192.237.4.0;
    option broadcast-address 192.237.4.255;
}

subnet 192.237.1.0 netmask 255.255.255.0 {
    range 192.237.1.5 192.237.1.25;
    range 192.237.1.50 192.237.1.100;
    option routers 192.237.1.1;
    option broadcast-address 192.237.1.255;
    option domain-name-servers 192.237.4.1;
    default-lease-time 1800;
    max-lease-time 5220;
}

subnet 192.237.2.0 netmask 255.255.255.0 {
    range 192.237.2.9 192.237.2.27;
    range 192.237.2.81 192.237.2.243;
    option routers 192.237.2.1;
    option broadcast-address 192.237.2.255;
    option domain-name-servers 192.237.4.1;
    default-lease-time 360;
    max-lease-time 5220;
} ' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

Test pada client
![Screenshot 2024-10-27 212306](https://github.com/user-attachments/assets/d7ba0b1d-bc32-4077-b8fe-6a0971e93db0)
![Screenshot 2024-10-27 212356](https://github.com/user-attachments/assets/1b270234-e03c-4cc9-9f3c-44592b1537df)

### Nomor 6

Menjalankan script di bawah ini kepada node PHP Worker (Armin, Eren, Mikasa) dan mengeceknya menggunakan `lynx localhost`.

```
#/bin/bash

wget 'https://drive.usercontent.google.com/u/0/uc?id=1RCy_7ptfsGXQ8_HJ8-28tts1a3EpnBlQ&export=download' -O modul3.zip
unzip modul3.zip
mkdir -p /var/www/eldia
mv modul-3/* /var/www/eldia

cat <<EOF >> /etc/nginx/sites-available/eldia.it08.com
server {
     listen 80;
     server_name _;

     root /var/www/eldia;
     index index.php index.html index.htm;

     location / {
         try_files $uri $uri/ /index.php?$query_string;
     }

     location ~ \.php$ {
         include snippets/fastcgi-php.conf;
         fastcgi_pass unix:/run/php/php7.3-fpm.sock;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         include fastcgi_params;
     }
}
EOF

ln -s /etc/nginx/sites-available/eldia.it08.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

### Nomor 11

Menjalankan script berikut 

```
#!/bin/bash

cat <<EOF >> /etc/nginx/sites-available/lb_php
upstream worker-aot {
    server 192.237.2.2;
    server 192.237.2.3;
    server 192.237.2.4;
}

server {
    listen 80;
    server_name eldia.it08.com www.eldia.it08.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        proxy_pass http://worker-aot;
    }

    location /titan {
        proxy_pass https://attackontitan.fandom.com/wiki/Attack_on_Titan_Wiki;
        proxy_set_header Host attackontitan.fandom.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
}
EOF

rm -f /etc/nginx/sites-enabled/lb_php
ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/

service nginx restart
```

### Nomor 12

Menjalankan script berikut

```
#!/bin/bash

cat <<EOF >> /etc/nginx/sites-available/lb_php
upstream worker-aot {
    server 192.237.2.2;
    server 192.237.2.3;
    server 192.237.2.4;
}

server {
    listen 80;
    server_name eldia.it08.com www.eldia.it08.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        allow 192.237.1.77;
        allow 192.237.1.88;
        allow 192.237.2.144;
        allow 192.237.2.156;
        deny all;
        proxy_pass http://worker-aot;
    }

    location /dune {
        proxy_pass https://attackontitan.fandom.com/wiki/Attack_on_Titan_Wiki;
        proxy_set_header Host attackontitan.fandom.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
}
EOF

rm -f /etc/nginx/sites-enabled/lb_php
ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/

service nginx restart
```

### Nomor 13

Menjalankan script berikut

```
#!/bin/bash

cat <<EOF >> /etc/mysql/my.cnf
# This group is read both by the client and the server
# use it for options that affect everything
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

# Options affecting the MySQL server (mysqld)
[mysqld]
skip-networking=0
skip-bind-address
EOF
```
Selanjutnya, jalankan command ini di terminal node Warhammer

```
mysql -u root -p
Enter password: (kosongkan password, langsung enter)

CREATE USER 'it08'@'%' IDENTIFIED BY 'it08';
CREATE USER 'it08'@'localhost' IDENTIFIED BY 'it08';
CREATE DATABASE db_it08;
GRANT ALL PRIVILEGES ON *.* TO 'it08'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'it08'@'localhost';
FLUSH PRIVILEGES;

mariadb --host=192.237.3.2 --port=3306 --user=it08 --password=it08 db_it08 -e "SHOW DATABASES;"
```

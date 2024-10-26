# Jarkom-Modul-3-IT08-2024

| Nama          | NRP          |
| ------------- | ------------ |
| Irfan Qobus Salim | 5027221058 |
| Aisha Ayya Ratiandari | 5027231056 |

## Topologi 2
![Screenshot 2024-10-21 225955](https://github.com/user-attachments/assets/7554a77f-d413-44d9-bdab-7766111e8509)

## Config

---

Config setiap node

### Paradis (DHCP Relay)

```markdown
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.237.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.237.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.237.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.237.4.0
	netmask 255.255.255.0

iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.237.0.0/16

```

### Tybur (DHCP Server)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.4.2
  netmask 255.255.255.0
  gateway 192.237.4.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Fritz (DNS Server)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.4.1
  netmask 255.255.255.0
  gateway 192.237.4.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Warhammer (database)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.3.3
  netmask 255.255.255.0
  gateway 192.237.3.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Beast (LoadBalancer Laravel)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.3.1
  netmask 255.255.255.0
  gateway 192.237.3.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Colossal (LoadBalancer PHP)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.3.2
  netmask 255.255.255.0
  gateway 192.237.3.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Annie (Laravel Worker)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.1.1
  netmask 255.255.255.0
  gateway 192.237.1.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Bertholdt (Laravel Worker)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.1.2
  netmask 255.255.255.0
  gateway 192.237.1.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Reiner (Laravel Worker)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.1.3
  netmask 255.255.255.0
  gateway 192.237.1.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Armin (PHP Worker)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.2.1
  netmask 255.255.255.0
  gateway 192.237.2.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Eren (PHP Worker)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.2.2
  netmask 255.255.255.0
  gateway 192.237.2.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Mikasa (PHP Worker)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.2.3
  netmask 255.255.255.0
  gateway 192.237.2.0

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Zeke (Client)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.1.4
  netmask 255.255.255.0
  gateway 192.237.1.0

up echo nameserver 192.237.4.1 > /etc/resolv.conf
up echo nameserver 192.168.122.1 >> /etc/resolv.conf
```

### Erwin (Client)

```markdown
auto eth0
iface  eth0 inet static
  address 192.237.2.4
  netmask 255.255.255.0
  gateway 192.237.2.0

up echo nameserver 192.237.4.1 > /etc/resolv.conf
up echo nameserver 192.168.122.1 >> /etc/resolv.conf

```

### Instalasi Dependensi

---

di nano /root/.bashrc per nodenya

**Paradis (DHCP Relay)**

```
apt-get update
apt-get install isc-dhcp-relay -y

```

**Tybur (DHCP Server)**

```
apt-get update
apt-get install isc-dhcp-server -y

```

**Fritz (DNS Server)**

```
apt-get update
apt-get install bind9 -y

```

**Beast dan Colossal (Load Balancer)**

```
apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

```

**Warhammer (Database Server)**

```
apt-get update
apt-get install mariadb-server -y

```

**Annie, Bertholdt, Reiner (Laravel Worker)**

**Armin, Eren, Mikasa (PHP Worker)**

**Zeke dan Erwin (Client)**

```
apt-get update
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install jq -y

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
@       IN    A    192.237.1.1
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
@       IN    A    192.237.2.1
"
echo "$granz" > /etc/bind/jarkom/eldia.it08.com

service bind9 restart
```

Test di client Zeke dan Erwin

![image.png](image%203.png)

![image.png](image%204.png)

![image.png](image%205.png)

![image.png](image%206.png)

### No. 1 - 5

---

Membuat script untuk menghubungkan Paradis ke Tybur

```markdown
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

relay="SERVERS=\"191.237.4.2\"
INTERFACES=\"eth1 eth2 eth3 eth4\"
OPTIONS=\"\"
"
echo "$relay" > /etc/default/isc-dhcp-relay

echo net.ipv4.ip_forward=1 > /etc/sysctl.conf

service isc-dhcp-relay restart
```

lalu menghubungkan Tybur untuk setup subnet Marley

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

# LaTeX-Pipeline Hub – Primary DNS Setup Documentation

## 1. Install bind9
Commands:
sudo apt update
sudo apt install bind9 bind9utils -y

## 2. Configure zones
File: /etc/bind/named.conf.local
---------------------------------
zone "ci.intern" {
    type master;
    file "/etc/bind/db.ci.intern";
    allow-transfer { 10.0.0.11; };
};

zone "0.0.10.in-addr.arpa" {
    type master;
    file "/etc/bind/db.10";
    allow-transfer { 10.0.0.11; };
};

File: /etc/bind/db.ci.intern
-----------------------------
$TTL 3600
@   IN  SOA primary.dns.ci.intern. admin.ci.intern. (
        2025010101
        3600
        1800
        604800
        86400 )

    IN  NS  primary.dns.ci.intern.
    IN  NS  secondary.dns.ci.intern.

primary.dns     IN A 10.0.0.10
secondary.dns   IN A 10.0.0.11
gitlab          IN A 10.0.0.20
runner          IN A 10.0.0.21

File: /etc/bind/db.10
----------------------
$TTL 3600
@   IN  SOA primary.dns.ci.intern. admin.ci.intern. (
        2025010101
        3600
        1800
        604800
        86400 )

    IN NS primary.dns.ci.intern.
    IN NS secondary.dns.ci.intern.

10  IN PTR primary.dns.ci.intern.
11  IN PTR secondary.dns.ci.intern.
20  IN PTR gitlab.ci.intern.
21  IN PTR runner.ci.intern.

## 3. Validate configuration
Commands:
sudo named-checkconf
sudo named-checkzone ci.intern /etc/bind/db.ci.intern
sudo named-checkzone 0.0.10.in-addr.arpa /etc/bind/db.10

## 4. Restart bind9
sudo systemctl restart bind9

## 5. DNS tests
Forward lookup:
dig gitlab.ci.intern @127.0.0.1

Reverse lookup:
dig -x 10.0.0.20 @127.0.0.1

Result:
Primary DNS resolved all entries correctly and is functioning as intended.

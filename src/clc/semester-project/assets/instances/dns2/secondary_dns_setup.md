# LaTeX-Pipeline Hub – Secondary DNS Setup Documentation

## 1. Install bind9
Commands:
sudo apt update
sudo apt install bind9 bind9utils -y

## 2. Configure slave zones
File: /etc/bind/named.conf.local
---------------------------------
zone "ci.intern" {
    type slave;
    masters { 10.0.0.10; };
    file "/var/cache/bind/db.ci.intern";
};

zone "0.0.10.in-addr.arpa" {
    type slave;
    masters { 10.0.0.10; };
    file "/var/cache/bind/db.10";
};

## 3. Restart bind9
Command:
sudo systemctl restart bind9

## 4. Test zone transfer
Command:
dig AXFR ci.intern @10.0.0.10

Expected result:
Full zone record list is transferred successfully from primary DNS.

## 5. DNS tests on secondary DNS
Forward lookup:
dig gitlab.ci.intern @127.0.0.1

Reverse lookup:
dig -x 10.0.0.20 @127.0.0.1

Expected result:
Secondary DNS resolves all records correctly, confirming zone synchronization.


## Bật SNMP trên switch cisco

```sh
enable
config terminal
snmp-server community public RO
snmp-server host 10.145.34.130 public
```

##Cấu hình tại Shinken host

Cài đặt plugin check_nwc_health

```sh
su - shinken
wget http://labs.consol.de/download/shinken-nagios-plugins/check_nwc_health-3.1.tar.gz
tar -xvf check_nwc_health-3.1.tar.gz
cd check_nwc_health-3.1/
./configure --prefix=/var/lib/shinken --with-nagios-user=shinken --with-nagios-group=shinken
make
make install
ll /var/lib/shinken/libexec/check_nwc_health
```

Cài các pack cần thiết

```sh
cd 
shinken install cisco
shinken install switch
```

Tạo host cho switch cần monitor

`vi /etc/shinken/hosts/switch3750.cfg`

    define host{
    host_name               switch3750
    address                 10.145.34.2
    use             switch,cisco
    _SNMPCOMMUNITY  public
    }

Restart shinken

`/etc/init.d/shinken restart`
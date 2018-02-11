This will show you how to install and run xmr-stak-cpu on Ubuntu 16.04.

#### Install XMR-Stak-CPU Required Packages

```
sudo apt-get --assume-yes update
sudo apt-get --assume-yes install libmicrohttpd-dev libssl-dev cmake build-essential libhwloc-dev screen git nano
cd /usr/local/src/
git clone https://github.com/fireice-uk/xmr-stak-cpu.git
cd xmr-stak-cpu
cmake .
make install

```
After make install runs you will need to change directories:

> cd bin/

Then you will run xmr-stak-cpu for the first time making sure it is executable:

> chmod +x xmr-stak-cpu

> ./xmr-stak-cpu

Yu will get threading config like below: 

```
**************** Copy&Paste BEGIN ****************
"cpu_threads_conf" :
[
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 0 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 1 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 2 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 3 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 4 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 5 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 6 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 7 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 8 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 9 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 10 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 11 },
    { "low_power_mode" : false, "no_prefetch" : true, "affine_to_cpu" : 12 },
],
**************** Copy&Paste END ****************

```

### Edit XMR-Stak-CPU Config File

Add Above thread conf and below pool details in config.txt

> vi config.txt

```
"pool_address": "pool.usxmrpool.com:3333",
"wallet_address": "43VXR67CfX5NcbXna9EZjwFnBhGrnqxc4Usw2CGxrGH8YaiUPfTsBacLgVcCmUrWGqdCVMsLDhbj1CDjNs5KgKcg98C8XH5",
"pool_password": "WorkerName:Email@EMail.com",
```

### Setup Huge Pages
If you run into issues related to mmap this means you need to enable hugepages. To do this type the following commands:

>sysctl -w vm.nr_hugepages=128
>vi /etc/sysctl.conf

At the end of the sysctl.conf file add:

> vm.nr_hugepages=128

### Add XMR-Stak-CPU As Service
If your server loses power or reboots for any reason, this will automatically start xmr-stak-cpu. Create a service file with:

> vi /lib/systemd/system/xmr.service

```
[Unit]
Description=xmr
After=network.target
[Service]
ExecStart=/usr/local/src/xmr-stak-cpu/bin/xmr-stak-cpu -c /usr/local/src/xmr-stak-cpu/bin/config.txt
User=root
[Install]
WantedBy=multi-user.target
```
Now run the following commands:

```
sudo systemctl daemon-reload
sudo systemctl enable xmr.service
```

To control the service:
```
sudo systemctl status xmr.service
sudo systemctl start xmr.service
sudo systemctl stop xmr.service
sudo systemctl restart xmr.service
```


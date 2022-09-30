# 1. k8s
k8s mong đượch hỗ trợ cài vì có nhiều câu lệnh root nó cần có các câu lệnh khác chạy trước.
## 1.1 thông tin server:
```
os_vda@10.205.58.1
os_vda@10.205.58.2
os_vda@10.205.58.3
os_vda@10.205.58.4
os_vda@10.205.58.5
os_vda@10.205.58.6
os_vda@10.205.58.7
os_vda@10.205.58.8
os_vda@10.205.58.9
os_vda@10.205.58.10
os_vda@10.205.58.11
os_vda@10.205.58.12
os_vda@10.205.58.13
os_vda@10.205.58.14
os_vda@10.205.58.15
```
## 1.2 câu lệnh cần chạy:
### 1.2.1 thêm host
```sh
echo '10.205.58.1  worker-k8s-07' | sudo tee -a /etc/hosts
echo '10.205.58.2  worker-k8s-08' | sudo tee -a /etc/hosts
echo '10.205.58.3  worker-k8s-09' | sudo tee -a /etc/hosts
echo '10.205.58.4  worker-k8s-10' | sudo tee -a /etc/hosts
echo '10.205.58.5  worker-k8s-11' | sudo tee -a /etc/hosts
echo '10.205.58.6  worker-k8s-12' | sudo tee -a /etc/hosts
echo '10.205.58.7  worker-k8s-13' | sudo tee -a /etc/hosts
echo '10.205.58.8  worker-k8s-14' | sudo tee -a /etc/hosts
echo '10.205.58.9  worker-k8s-15' | sudo tee -a /etc/hosts
echo '10.205.58.10  worker-k8s-16' | sudo tee -a /etc/hosts
echo '10.205.58.11  worker-k8s-17' | sudo tee -a /etc/hosts
echo '10.205.58.12  worker-k8s-18' | sudo tee -a /etc/hosts
echo '10.205.58.13  worker-k8s-19' | sudo tee -a /etc/hosts
echo '10.205.58.14  worker-k8s-20' | sudo tee -a /etc/hosts
echo '10.205.58.15  worker-k8s-21' | sudo tee -a /etc/hosts
```
### 1.2.1 cài đặt 
```bash
yum install libtool-ltdl-2.4.2-22.el7_3.x86_64.rpm
yum install pigz-2.3.3-1.el7.centos.x86_64.rpm 
yum install container-selinux-2.119.2-1.911c772.el7_8.noarch.rpm 
yum install containerd.io-1.4.3-3.2.el7.x86_64.rpm 
yum install docker-ce-cli-19.03.15-3.el7.x86_64.rpm 
yum install docker-ce-19.03.15-3.el7.x86_64.rpm 
```
### 1.2.2 Setup daemon
```sh
cat > /etc/docker/daemon.json <<EOF
{
  "insecure-registries": ["10.207.58.2:5000"],
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
EOF
```
###  1.2.3 Start docker bằng câu lệnh sau
```sh
sudo systemctl start docker
```

### 1.2.4 Cấp quyền cho user sử dụng docker
```sh
sudo groupadd docker
sudo usermod -aG docker ${USER}
```
1.2.5 ...
# 2. mysql
## 2.1 thông tin server:
```
os_vda@10.205.58.16
os_vda@10.205.58.17
os_vda@10.205.58.18
```
## 2.2 câu lệnh cần chạy:
### 2.2.1 Disable selinux
```sh
vi /etc/selinux/config
```
SELINUX=disabled
### 2.2.2 thêm hosts:
```sh
echo '10.205.58.16  vda-mysql-04' | sudo tee -a /etc/hosts
echo '10.205.58.17  vda-mysql-05' | sudo tee -a /etc/hosts
echo '10.205.58.18  vda-mysql-06' | sudo tee -a /etc/hosts
```
# 3. elasticsearch
## 3.1 thông tin server:
```
os_vda@10.205.58.19
os_vda@10.205.58.20
os_vda@10.205.58.21
os_vda@10.205.58.22
os_vda@10.205.58.23
os_vda@10.205.58.24
```
## 3.2 câu lệnh cần chạy:
### 3.2.1: thêm host
```sh
echo '10.205.58.19  vda-es-04' | sudo tee -a /etc/hosts
echo '10.205.58.20  vda-es-05' | sudo tee -a /etc/hosts
echo '10.205.58.21  vda-es-06' | sudo tee -a /etc/hosts
echo '10.205.58.22  vda-es-07' | sudo tee -a /etc/hosts
echo '10.205.58.23  vda-es-08' | sudo tee -a /etc/hosts
echo '10.205.58.24  vda-es-09' | sudo tee -a /etc/hosts
```

# 4. redis
## 4.1 thông tin server:
```
os_vda@10.205.58.25
os_vda@10.205.58.26
os_vda@10.205.58.27
```
## 4.2 câu lệnh cần chạy:
### 4.2.1 thêm host :
```sh
echo '10.205.58.25  vda-redis-04' | sudo tee -a /etc/hosts
echo '10.205.58.26  vda-redis-05' | sudo tee -a /etc/hosts
echo '10.205.58.37  vda-redis-06' | sudo tee -a /etc/hosts
```
### 4.2.2 cài đặt redis
```sh
tar -zxvf redis-5.0.5.tar.gz
cd redis-5.0.5
sudo make
sudo make install
which redis-server
```
### 4.2.3 thay đổi tham số
```sh
echo 65535 > /proc/sys/net/core/somaxconn
echo "net.core.somaxconn=65535" >> /etc/sysctl.conf
echo 65535 > /proc/sys/net/ipv4/tcp_max_syn_backlog
echo "net.ipv4.tcp_max_syn_backlog=65535" >> /etc/sysctl.conf
sysctl vm.overcommit_memory=1
echo "vm.overcommit_memory=1" >> /etc/sysctl.conf
echo never >> /sys/kernel/mm/transparent_hugepage/enabled
echo "echo never >> /sys/kernel/mm/transparent_hugepage/enabled" >> /etc/rc.local
sysctl vm.swappiness=0
echo "sysctl vm.swappiness=0" >> /etc/sysctl.conf
```
# 5. kafka
## 5.1 thông tin server:
```
os_vda@10.205.58.28
os_vda@10.205.58.29
os_vda@10.205.58.30
os_vda@10.205.58.31
os_vda@10.205.58.32
os_vda@10.205.58.33
```
## 5.2 câu lệnh cần chạy:
### 5.2.1: thêm host
```sh
echo '10.205.58.28  vda-kafka-04' | sudo tee -a /etc/hosts
echo '10.205.58.29  vda-kafka-05' | sudo tee -a /etc/hosts
echo '10.205.58.30  vda-kafka-06' | sudo tee -a /etc/hosts
echo '10.205.58.31  vda-kafka-07' | sudo tee -a /etc/hosts
echo '10.205.58.32  vda-kafka-08' | sudo tee -a /etc/hosts
echo '10.205.58.33  vda-kafka-09' | sudo tee -a /etc/hosts
```
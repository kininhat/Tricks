# iptables

## I. Lấy bytescode

### 1. Thực hiện qua tcpdump bằng cách thêm card mạng ảo trên thiết bị các nhân (linux/mac)

```bash
    sudo ip tuntap add tun0 mode tun
    sudo ip link set tun0 up
    sudo tcpdump -ddd -i tun0 ip proto 6
```

* filter ESP protocol 0x32 hoặc thay bằng thành phần tương tự

```bash
sudo tcpdump -ddd -i tun0 'ip[9]=0x32' | tr '\n' ','
sudo tcpdump -ddd -i tun0 'tcp[13]=0x0c2' and 'ip[1]>0' 
```

## II. 1 số rules thông dụng

### 1.1 cpanel

### 1.1.1 syn && ip[1]>0 & ip[2:2]>=60 & cpanel - chặn nhầm qua cloudflare nhưng rất ít

iptables -t raw -I PREROUTING -p tcp --syn -i bond0+ -m multiport --dports 80,443,2087,2083 -m bpf --bytecode '9,48 0 0 0,84 0 0 240,21 0 5 64,48 0 0 1,37 0 3 0,40 0 0 2,53 0 1 60,6 0 0 262144,6 0 0 0' -m comment --comment "tos>0 ip lenth >=60" -j DROP

### 1.1.2 syn && ip[1]=0 & ip[2:2]>=60 & cpanel -> rules này có khả năng chặn nhầm 1 ít website

iptables -t raw -I PREROUTING -p tcp --syn -i bond0+ -m multiport --dports 80,443,2087,2083 -m bpf --bytecode '9,48 0 0 0,84 0 0 240,21 0 5 64,48 0 0 1,21 0 3 0,40 0 0 2,53 0 1 60,6 0 0 262144,6 0 0 0' -m comment --comment "tos=0 ip lenth >=60" -j DROP

### 1.1.3 syn && ip[1]=0 & ip[2:2]=60 & cpanel

iptables -t raw -I PREROUTING -p tcp --syn -i bond0+ -m multiport --dports 80,443,2087,2083 -m bpf --bytecode '9,48 0 0 0,84 0 0 240,21 0 5 64,40 0 0 2,21 0 3 60,48 0 0 1,21 0 1 0,6 0 0 262144,6 0 0 0' -m ttl --ttl-eq 64 -m comment --comment "tos=0 ip lenth >=60 & ttl=64" -j DROP

## 2. webserver port 80

### 2.1 get /?

iptables -t raw -I   WEB -p tcp --dport 80 -m string --hex-string "|4745 5420 2f3f|" --algo kmp --to 65535 -j SET --add-set BLACKLIST_SrcDstportDst src,dst,dst --timeout 21600
iptables -t raw -I   WEB -p tcp --dport 80 -m string --hex-string "|4745 5420 2f3f|" --algo kmp --to 65535 -j SET --add-set BLACKLIST_SrcDstportDst src,dst,dst --timeout 21600

### 2.2 head /?

iptables -t raw -I   WEB -p tcp --dport 80 -m string --hex-string "|4845 4144 202f 3f|" --algo kmp --to 65535 -j SET --add-set BLACKLIST_SrcDstportDst src,dst,dst --timeout 21600

### 2.3 post /?

iptables -t raw -I   WEB -p tcp --dport 80 -m string --hex-string "|504f 5354 202f 3f|" --algo kmp --to 65535 -j SET --add-set BLACKLIST_SrcDstportDst src,dst,dst --timeout 21600

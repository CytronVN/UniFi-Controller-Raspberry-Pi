# UniFi-Controller-Raspberry-Pi
Cài đặt UniFi Controller (UniFi Network) trên Raspberry Pi chạy Raspberry Pi OS

Đầu tiên, bạn cần cài đặt Raspberry Pi OS lên thẻ nhớ và bật SSH với phần mềm Raspberry Pi Imager. Ngoài ra, nếu sử dụng kết nối WiFi với Raspberry Pi, bạn cũng có thể cấu hình nó với Pi Imager.
Chúng ta sẽ cần đặt IP tĩnh cho Raspberry Pi. Bạn có thể chỉnh sửa file /etc/dhcpcd.conf hoặc sử dụng giao diện desktop.

```sudo nano /etc/dhcpcd.conf```

```
#Static IP configuration:
interface eth0
static ip_address=192.168.1.10/24
#static ip6_address=fd51:42f8:caae:d92e::ff/64
static routers=192.168.1.1
static domain_name_servers=192.168.1.1 1.1.1.1 fd51:42f8:caae:d92e::1
```
Trong file ví dụ trên, địa chỉ IP tĩnh của Raspberry Pi là 192.168.1.10 và gateway (IP LAN của router) là 192.168.1.1

Sau khi chỉnh sửa xong, hãy khởi động lại Raspberry Pi với lệnh reboot.

Bước tiếp theo, chúng ta cần cập nhật Raspberry Pi OS

```sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get autoremove && sudo apt-get autoclean```

Sau đó là cài đặt Java

```sudo apt-get install openjdk-8-jre-headless -y```

Với dòng lệnh này, bạn sẽ thêm repository của UniFi vào RPI OS

```
echo 'deb http://www.ui.com/downloads/unifi/debian stable ubiquiti' | sudo tee /etc/apt/sources.list.d/100-ubnt-unifi.list
```
Và tiếp theo là chỉnh sửa phiên bản của MongoDB 

```
echo 'deb http://archive.raspbian.org/raspbian stretch main contrib non-free rpi' | sudo tee /etc/apt/sources.list.d/raspbian_stretch_for_mongodb.list
```

Để có thể cập nhật được repo, bạn cần thêm GPG key của UniFi vào RPI OS

```
sudo wget -O /etc/apt/trusted.gpg.d/unifi-repo.gpg https://dl.ubnt.com/unifi/unifi-repo.gpg
```

Giờ đây, bạn đã có thể cài đặt UniFi Controller với apt:

```
sudo apt-get update; sudo apt-get install unifi -y
```

Sau đó, chúng ta có thể gỡ database mặc định được cài với MongdoDB với dòng lệnh

```
sudo systemctl stop mongodb 
sudo systemctl disable mongodb
```

Và sau cùng là khởi động lại Raspberry Pi
```
sudo reboot
```
Giờ đây, bạn có thể truy cập vào UniFi Controller tại IP:8443 hoặc raspberrypi:8443 để quản lý các thiết bị UniFi của mình.





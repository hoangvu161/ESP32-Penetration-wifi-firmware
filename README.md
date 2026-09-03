This file contains 2 languages(EN & VI)
Looking for the Vietnamese version? Just SCROLL DOWN bro!
#######################################ENGLISH(en)#############################################
ESP32 Wi-Fi Penetration Tool Firmware

* Features:

* Auto-activating Headless mode (standalone Web UI)
* PMKID, Handshake, DoS attack types
* Deauth broadcast (active), Only capture (passive), Rogue AP (passive)

* Power Consumption:

* Average power consumption during attacks is ~100mA (varies by board model); maximum allowable current draw is 300mA.

* Note:

* Designed specifically for ESP32 boards WITHOUT SD card slots and screens (OLED, TFT, etc.).
* Typically ONLY works on ESP32-WROOM boards and similar modules (e.g., ESP32-D0WD). Other board variants are UNTESTED and may cause soft-bricks (boot loops, partition table corruption, etc.).
```
████████╗██╗   ██╗████████╗██████╗ ██████╗ ██╗ █████╗ ██╗
╚══██╔══╝██║   ██║╚══██╔══╝██╔═══██╗██╔══██╗██║██╔══██╗██║
   ██║   ██║   ██║   ██║   ██║   ██║██████╔╝██║███████║██║
   ██║   ██║   ██║   ██║   ██║   ██║██╔══██╗██║██╔══██║██║
   ██║   ╚██████╔╝   ██║   ╚██████╔╝██║  ██║██║██║  ██║███████╗
   ╚═╝    ╚═════╝    ╚═╝    ╚═════╝ ╚═╝  ╚═╝╚═╝╚═╝  ╚═╝╚══════╝
--------------------------- TUTORIAL ---------------------------
```
> **WARNING:** DO NOT flash on ESP32-S2, ESP32-S3, ESP32-C3, or other custom board variants unless explicitly tested. Flashing incompatible firmware may cause boot loops or partition table corruption in Flash ROM.

## 1. Installing `esptool`

There are two primary methods to install the `esptool` flashing tool. **Method 1 (`pipx`)** is strongly recommended to prevent missing flasher stub dependencies.

### METHOD 1: Installation via `pipx` (EXTREMELY Recommended)

On modern Linux distributions (Debian 12/13, Arch, Ubuntu 24.04+), the system Python environment is externally managed per PEP 668. Using `pipx` isolates `esptool` inside its own virtual environment while retaining global CLI access.

1. **Install `pipx` and Python dependencies:**
   sudo apt update && sudo apt install -y python3-pip python3-venv pipx
   pipx ensurepath

*(Note: If running `ensurepath` for the first time, restart your terminal session or run `source ~/.bashrc`)*

2. **Install `esptool`:**

pipx install esptool

3. *(Optional)* **Verify installation:**

esptool.py version

### METHOD 2: Installation via `apt` (NOT recommended for Debian 13 Trixie)

sudo apt update && sudo apt install -y esptool

> **IMPORTANT CAVEAT FOR DEBIAN 13 (TRIXIE):**
> The `esptool` package in Debian 13 Trixie's APT repository currently suffers from a packaging defect (missing RAM flasher stub binaries). During UART programming, `esptool` installed via `apt` will fail to upload the initial RAM stub (resulting in `A fatal error occurred: Failed to connect...` or hanging indefinitely at `Uploading stub...`).
> **=> If you are running Debian 13 Trixie, use Method 1 (`pipx`).**

## 2. Flash Firmware

### Step 1: Serial Port Permissions

Add your current user to the `dialout` (or `tty`) group to interact with `/dev/ttyUSB0` or `/dev/ttyACM0` without root privileges:

sudo usermod -aG dialout $USER

*(Requires logging out and back in or rebooting to apply group membership).*

### Step 2: Identify ESP32 Serial Port

Connect your ESP32 board and locate the assigned device path:

ls -l /dev/ttyUSB* /dev/ttyACM* 2>/dev/null

# Or inspect kernel output:

sudo dmesg | grep -i tty

### Step 3: Erase Flash Memory

Full flash erasure is recommended to prevent partition table conflicts:

esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 921600 erase_flash

*(Replace `/dev/ttyUSB0` with your target device port).*

### Step 4: Write Firmware Images

esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 921600 write_flash -z \
  0x1000 bootloader.bin \
  0x8000 partitions.bin \
  0x10000 boot_app0.bin \
  0x20000 firmware.bin

## 3. Usage & Connection

1. Upon successful flashing, press the **RST / RESET** button on your ESP32 board (or cycle power).
2. The ESP32 will boot in Headless Mode and spin up an Access Point (AP).
3. Connect your mobile device or laptop to the target Wi-Fi AP:
* **SSID: `ManagementAP`
* **Password: `mgmtadmin`


4. Open a web browser and navigate directly to:
`http://192.168.4.1`

###############################-----------Disclaimer-----------###############################

* This firmware is packaged and provided EXCLUSIVELY for educational, research, and authorized security testing purposes. The author assumes NO responsibility for unauthorized usage or unlawful activities.

#########################-----------Credits & Acknowledgments-----------#########################

SUPER THANKS to Original Author / Core Firmware: [risinek](https://github.com/risinek) – Author of the original "esp32-wifi-penetration-tool" project.

Packager / Maintainer: HoangVu(https://github.com/hoangvu161) – Headless configuration optimization and installation package maintenance.

#######################################Vietnamese(vi)#############################################

Firmware kiểm thử bảo mật wi-fi cho esp32(wifi penetration tool)

+Tính năng:

-Tự động active Headless mode(sử dụng WEB_UI độc lập)
-Pmkid, handshake, DOS attack type
-Deauth broadcast(active), only capture(passive), rogue_ap(passive)

+Mức tiêu thụ điện:

-Trong quá trình thực hiện attack, esp32 tiêu thụ khoảng 100mA(tùy loại sẽ có các mức tiêu thụ khác nhau), mức tiêu thụ điện tối đa(peak) cho phép là 300mA

*Lưu ý:

-Firmware dành cho các board esp32 KHÔNG sd, KHÔNG màn hình(oled,TFT,..)
-Có thể dùng các loại Micro:bit, yolo:bit
-Thường CHỈ hoạt động trên các board dạng ESP32-WROOM và các loại tương tự(DOWD), các loại board khác CHƯA được thử nghiệm và có khả năng làm HỎNG board của bạn(treo boot, lỗi phân vùng,..)
```
████████╗██╗   ██╗████████╗██████╗ ██████╗ ██╗ █████╗ ██╗
╚══██╔══╝██║   ██║╚══██╔══╝██╔═══██╗██╔══██╗██║██╔══██╗██║
   ██║   ██║   ██║   ██║   ██║   ██║██████╔╝██║███████║██║
   ██║   ██║   ██║   ██║   ██║   ██║██╔══██╗██║██╔══██║██║
   ██║   ╚██████╔╝   ██║   ╚██████╔╝██║  ██║██║██║  ██║███████╗
   ╚═╝    ╚═════╝    ╚═╝    ╚═════╝ ╚═╝  ╚═╝╚═╝╚═╝  ╚═╝╚══════╝
--------------------------HƯỚNG DẪN----------------------------
```
> **LƯU Ý:** KHÔNG nạp trên các dòng chip ESP32-S2, ESP32-S3, ESP32-C3 hoặc các board tùy biến khác nếu CHƯA kiểm thử. Việc flash sai firmware có thể gây treo bootloop hoặc làm hỏng phân vùng Flash ROM.

## Cài esptool

Có 2 cách cài đặt công cụ nạp esptool. Khuyến khích sử dụng **Cách 1 (pipx)** để tránh lỗi thư viện stub.

### Cách 1: Cài đặt qua pipx (Khuyên dùng)

Trên các bản Linux hiện đại (Debian 12/13, Arch, Ubuntu 24.04+), môi trường Python hệ thống bị khóa bởi PEP 668 (externally-managed-environment). Dùng pipx giúp cô lập esptool vào môi trường venv riêng mà vẫn gọi được global command.

1.1 **Cài đặt pipx và môi trường Python:**

sudo apt update

sudo apt install -y python3-pip python3-venv pipx

pipx ensurepath

*(LƯU Ý: Nếu mới chạy ensurepath lần đầu, hãy restart terminal hoặc source ~/.bashrc)

1.2. **Cài esptool:**

pipx install esptool

1.3(OPTIONAL). **Kiểm tra phiên bản:**

esptool.py version


### Cách 2: Cài qua apt (KHÔNG nên thực hiện trên Debian 13 Trixie)

sudo apt update && sudo apt install esptool -y


> **THÔNG TIN THỬ NGHIỆM THỰC TẾ ĐỐI VỚI CÁCH 2 TRÊN DEBIAN 13 (TRIXIE):**
> Gói esptool phân phối trên kho APT của Debian 13 Trixie hiện đang bị lỗi đóng gói (missing flasher stub binary files). Khi thực hiện giao tiếp nạp qua UART, esptool từ apt có thể báo lỗi không upload được file mồi RAM stub (gây ra lỗi A fatal error occurred: Failed to connect... hoặc đứng ở bước `Uploading stub...`).
> **=> Nếu bạn đang dùng Debian 13 Trixie, tui KHUYÊN bạn nên dùng CÁCH 1 (pipx).**

## 3. Flash Firmware

### 1: Phân quyền truy cập cho cổng Serial

Thêm user hiện tại vào group dialout (hoặc tty) để hoạt động với /dev/ttyUSB0 hoặc /dev/ttyACM0 không cần sudo:

sudo usermod -aG dialout $USER

*(Cần log out và log in lại/reboot để nhận group mới).*

### 2: Xác định cổng kết nối ESP32

Cắm board ESP32 vào máy và check thiết bị nhận diện:

ls -l /dev/ttyUSB* /dev/ttyACM* 2>/dev/null

# Hoặc check dmesg:

sudo dmesg | grep -i tty

### 3: Xóa sạch Flash cũ (Erase Flash)

-Bạn nên xóa sạch dữ liệu ROM cũ để tránh xung đột bảng phân vùng (partition table):

esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 921600 erase_flash

*(Thay /dev/ttyUSB0 bằng cổng thiết bị của bạn).*

### 4: Nạp Firmware

esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 921600 write_flash -z \
  0x1000 bootloader.bin \
  0x8000 partitions.bin \
  0x10000 boot_app0.bin \
  0x20000 firmware.bin

## 4. Vận hành thiết bị & Kết nối (Usage)

1. Sau khi nạp xong, nhấn nút **RST / RESET** trên board ESP32 (hoặc rút nguồn cắm lại).
2. ESP32 sẽ khởi động ở chế độ Headless và phát một Access Point (AP).
3. Sử dụng điện thoại hoặc máy tính kết nối vào Wi-Fi của ESP32.
*Tên wifi (ssid) : ManagementAP | Pass : mgmtadmin
4. Truy cập địa chỉ IP trên trình duyệt để vào Web UI:

http://192.168.4.1 (Nhập thẳng 192.168.4.1 cho nhanh)

###############################-----------Disclaimer-----------###############################

-Firmware này được đóng gói và cung cấp DUY NHẤT cho mục đích nghiên cứu giáo dục, học tập và kiểm thử bảo mật trong điều kiện CHO PHÉP. Tác giả KHÔNG chịu bất kỳ trách nhiệm nào đối với các hành vi sử dụng trái phép hoặc vi phạm pháp luật.

#########################-----------Credits & Acknowledgments-----------#########################

SUPER THANKS to Original Author / Core Firmware: [risinek](https://github.com/risinek) – Tác giả dự án gốc "esp32-wifi-penetration-tool".

Packager / Maintainer: HoangVu(https://github.com/hoangvu161) – Tối ưu hóa cấu hình headless và đóng gói bộ file cài đặt.
```
########################################### 
###########################################
#####################*#####################
####################***####################
###############*************###############
################***********################
#################*********#################
################***#####***################
###############**#########**###############
###########################################
###########################################

-Mừng ngày Quốc Khánh Việt Nam!
    ____  /  ___ 
   |___ \/  / _ \
     __) / | (_) |
    / __/  \__, |
   |____|    /_/
Last Updated: 02/09/2026

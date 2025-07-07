# CanopiCamera

Dự án khởi tạo thiết bị **Canopi Camera** trên nền tảng CM3588.

---

## Build file `.dtb` cho board Camera-CM3588

### 🔧 Các bước thực hiện:

#### **Bước 1**: Tạo thư mục chứa các file source `.dts` và `.dtb`

```bash
mkdir CameraDTB
```

#### **Bước 2**: Di chuyển đến thư mục chứa các file `.dtb` của kernel

```bash
cd /boot/dtb/rockchip
```

#### **Bước 3**: Copy file `rk3588-friendly-naskit-CM3588.dtb` vào thư mục mới tạo

```bash
cp rk3588-friendly-naskit-CM3588.dtb ~/Desktop/CameraDTB
```

#### **Bước 4**: Biên dịch ngược `.dtb` thành `.dts` để chỉnh sửa

```bash
dtc -I dtb -O dts -o rk3588-camera-build.dts rk3588-friendly-naskit-CM3588.dtb
```

#### **Bước 5**: Chỉnh sửa file `.dts` bằng VSCode

> 📌 **Ghi chú:**  
> Sau quá trình debug, nhận thấy hệ điều hành không khởi động do lỗi liên quan đến các cổng **PCIe**.  
> ✅ **Giải pháp:** Disable các cổng không sử dụng trong file `.dts`.  
> ✅ **Kết quả:** Hệ điều hành khởi động thành công.

---

## 💡 Cấu hình PCIe sau khi sửa đổi

### 🔹 Bring-up Hệ điều hành (OS)

| Port              | Trạng thái  |
|-------------------|-------------|
| pcie@fe180000     | `okay`      |
| pcie@fe1c0000     | `disabled`  |
| pcie@fe200000     | `disabled`  |
| pcie@fe240000     | `disabled`  |
| pcie@fe280000     | `disabled`  |

### 🔹 Bring-up Hệ điều hành + Ethernet

| Port              | Trạng thái  |
|-------------------|-------------|
| pcie@fe180000     | `okay`      |
| pcie@fe1c0000     | `okay`      |
| pcie@fe200000     | `disabled`  |
| pcie@fe240000     | `disabled`  |
| pcie@fe280000     | `disabled`  |

---

### 🚀 Hoàn tất quá trình build và áp dụng `.dtb`

#### **Bước 6**: Build lại file `.dtb` từ file `.dts` đã chỉnh sửa

```bash
dtc -I dts -O dtb -o rk3588-camera-build.dtb rk3588-camera-build.dts
```

#### **Bước 7**: Copy file `.dtb` mới vào thư mục `/boot/dtb/rockchip`

```bash
cp rk3588-camera-build.dtb /boot/dtb/rockchip
```

#### **Bước 8**: Hiệu chỉnh file `armbianEnv.txt` để chọn file `.dtb` khi boot

Mở file `/boot/armbianEnv.txt` và sửa dòng:

```ini
fdtfile=rockchip/rk3588-friendly-naskit-CM3588.dtb
```

thành:

```ini
fdtfile=rockchip/rk3588-camera-build.dtb
```

#### **Bước 9**: Khởi động lại thiết bị

```bash
sudo reboot
```

Hoặc tắt nguồn và bật lại thiết bị thủ công.

---

✅ *Hoàn thành bước bring-up thành công hệ điều hành và thiết bị mạng trên CM3588.*

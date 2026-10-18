# 🚀 Smart-Farm – Tưới tự động Trang trại Lan

## 📖 Mô tả

**Smart-Farm** là hệ thống IoT *tự đo độ ẩm, nhiệt độ và tự bật máy bơm*, giúp nông dân không phải ra vườn kiểm tra.

## 🏗️ Kiến trúc (IPO)

![Sơ đồ phần cứng Smart-Farm: cảm biến, ADC, Raspberry Pi, rơ-le, máy bơm][sysdiagram]

| Thành phần | Thiết bị |
|:--|:--|
| **Input** | Cảm biến độ ẩm, nhiệt độ + ADC `MCP3008` |
| **Process** | **SoC** Raspberry Pi (CPU + RAM + GPIO trên 1 chip) |
| **Output** | Rơ-le `GPIO17` → máy bơm |

## ⚙️ Cài đặt

```bash
git clone https://github.com/<ten-ban>/Smart-Farm.git
cd Smart-Farm && python3 pump_control.py
```

## ✅ Tính năng

- [x] Đo độ ẩm, nhiệt độ mỗi 60 giây
- [x] Tự bật bơm khi đất khô, tự tắt sau 10 giây
- [ ] Cảnh báo qua điện thoại

## 📊 Bảng thông số

| Thông số | Giá trị |
|:--|:--:|
| Vi xử lý | Raspberry Pi 4 (SoC) |
| Điện áp cảm biến | 3.3V |
| Ngưỡng tưới | Độ ẩm < 30% hoặc > 35°C |

## 🔄 Lệnh "tưới Lan": Fetch–Decode–Execute

> **Fetch:** CPU lấy lệnh `pump.on()` từ RAM.
>
> **Decode:** hiểu là đặt `GPIO17` lên mức cao.
>
> **Execute:** rơ-le đóng → máy bơm chạy.

## 🔺 DIKW

```text
      /\
     /W \     Wisdom: bật bơm 10 giây
    /----\
   /  K   \   Knowledge: nóng + đất khô = cần nước
  /--------\
 /    I     \ Information: nhiệt độ cao, độ ẩm thấp
/------------\
|     D      | Data: 36.5 (°C), 0.72
|____________|
```

## ⚠️ Lưu ý: RAM / bộ nhớ đầy nhật ký

> Log đầy có thể làm hệ thống treo, **bơm không tắt kịp**.

1. Kiểm tra bằng `free -h` và `df -h`.
2. Dọn log: `sudo journalctl --vacuum-size=50M`.
3. Giới hạn log 7 ngày bằng `logrotate`.
4. Cảm biến lỗi hoặc hệ thống treo thì **tắt bơm trước**.
5. Tắt máy bằng `sudo shutdown now`, không rút nguồn đột ngột.


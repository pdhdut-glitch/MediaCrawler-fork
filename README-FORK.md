# MediaCrawler — Fork ViralCrawl

Fork của [NanmiCoder/MediaCrawler](https://github.com/NanmiCoder/MediaCrawler) dùng làm
**thư viện dự phòng cào video** cho app XPipe (và tool ViralCrawl/XScaper trước đó).

> ⚠️ Giấy phép: thừa kế **NON-COMMERCIAL LEARNING LICENSE 1.1** từ dự án gốc —
> chỉ dùng cho mục đích học tập, KHÔNG thương mại. Xem `LICENSE`.

## Các thay đổi so với bản gốc

### Cấu hình qua biến môi trường (không phải sửa code)
| Env | Ý nghĩa |
|---|---|
| `MC_DATA_DIR` | thư mục gốc lưu data/video (gốc ghi cứng `data/`) |
| `MC_GET_MEDIAS=0` | chỉ lấy metadata, không tải media (xem trước) |
| `MC_XHS_INTL=1` | dùng Xiaohongshu quốc tế (rednote) thay vì nội địa |
| `MC_XHS_TIMEOUT` | timeout request XHS (giây, mặc định 60) |
| `MC_XHS_PROFILE` | profile browser riêng cho rednote |
| `MC_ENABLE_CDP`, `MC_CDP_PORT`, `MC_CDP_HEADLESS`, `MC_CDP_CONNECT_EXISTING` | chế độ CDP giữ browser persistent (chống 风控 cho Douyin) |
| `VC_PROXY`, `VC_PROXY_PROVIDER`, `VC_STATIC_PROXY` | gắn proxy theo từng máy |
| `VC_SLEEP_SEC` | nghỉ giữa các request |

### Sửa bug của bản gốc
- **Bỏ hardcode Redis** trong proxy pool — bản gốc crash `ConnectionError 10061` trên máy không có Redis.
- **Fix CDP websocket** — URL `/devtools/browser` thiếu id → 404.
- **XHS**: phân biệt lỗi `-104` (không có quyền) với chưa đăng nhập (qua pong code); khớp UA lúc login (phiên XHS gắn theo UA).
- **Douyin**: bắt kịp thay đổi URL trang kênh (`modal_id` → `vid`).
- Bỏ phụ thuộc `typing_extensions` (Python 3.9+ đủ).

### Tối ưu video
- **`tools/sua_hevc.py`**: HEVC/H.265 → H.264 — bản gốc tải video Douyin 2K dạng H.265 mà nhiều máy không decode được (màn đen, chỉ có tiếng). Converter không bao giờ làm mất video khi không probe được.
- Chọn gear tải ưu tiên **độ phân giải** thay vì ép codec.
- **rednote** dùng profile + thư mục data riêng, tách khỏi xhs nội địa.

## Cài đặt

```bash
python -m venv .venv
.venv/Scripts/pip install -r requirements.txt   # Windows
.venv/Scripts/playwright install chromium
```

Được app XPipe tự cài (tab Cài đặt → Thư viện) bằng cách clone repo này.

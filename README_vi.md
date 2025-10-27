# PharmApp — Trình tải & phát media kèm phụ đề (EN/VI)

App Tkinter gọn nhẹ, đa nền tảng để **tải MP3/MP4 cạnh file phụ đề `.vtt` bằng `yt-dlp`** và **phát media hiển thị song ngữ (EN/VI)** theo thời gian thực. Giao diện tối ưu chiều dọc theo **PharmApp theme**.

> Phiên bản hiện tại: **v3.1**  
> Hỗ trợ: **Windows / macOS / Linux**

---

## ✨ Tính năng

- **Tab Download**
  - Quét đệ quy thư mục gốc để tìm **`.vtt`**.
  - Tải **MP3/MP4** đặt **cạnh file phụ đề** (trùng tên; **bỏ qua nếu đã tồn tại**).
  - Chống lỗi **403**: dùng `youtube:player_client=android`, và fallback qua CLI với `--cookies-from-browser` (Chrome/Edge/Firefox/Brave).

- **Tab Player**
  - Phát **MP3/MP4** với **phụ đề song ngữ** (EN trên, VI dưới) đồng bộ theo thời gian phát.
  - Dùng `python-vlc` + VLC runtime, nhúng player trong cửa sổ.
  - Nút **Download Missing** nếu thiếu media.

- **Tab Subtitle Search → URL**
  - Tìm phụ đề theo tiêu đề hoặc `[YouTubeID]`.
  - Tạo **danh sách URL YouTube**, **Copy** vào clipboard hoặc **Export** ra `url_yt.txt`.

- **Tab Help** + **Footer 1 dòng** (link nhanh + nút **Donate**).
- Làm việc tốt với tên file Unicode & cây thư mục sâu.
- UI gọn, tiết kiệm chiều dọc.

---

## 📦 Cấu trúc dự án

App 1 file (có thể đổi tên tuỳ ý):

```
Subtitle_Media_Fetcher_v3.py
nct_logo.png        # icon cửa sổ (PNG)
nct_logo.ico        # icon cho Windows .exe
cookies.txt         # tuỳ chọn: cookies xuất từ trình duyệt (cho yt-dlp)
```

**Khuyến nghị đặt tên phụ đề** (để MP3/MP4 nằm cạnh dễ dàng):

```
Bài học nghiêm khắc từ Covid-19 [V4D3zeW_slg] - 2021-02-01.vi.vtt
Bài học nghiêm khắc từ Covid-19 [V4D3zeW_slg] - 2021-02-01.en.vtt
# Sau khi tải:
Bài học nghiêm khắc từ Covid-19 [V4D3zeW_slg] - 2021-02-01.vi.mp3
Bài học nghiêm khắc từ Covid-19 [V4D3zeW_slg] - 2021-02-01.vi.mp4
```

App trích **YouTube ID** từ mẫu tên `[...]` (ví dụ `[V4D3zeW_slg]`).

---

## 🚀 Cài đặt nhanh

```bash
# 1) Cài package Python
pip install yt-dlp python-vlc

# 2) Cài ffmpeg
#   Windows: winget install Gyan.FFmpeg
#   macOS:   brew install ffmpeg
#   Linux:   sudo apt-get install ffmpeg

# 3) Cài VLC (runtime cho python-vlc)
#   Windows/macOS: tải từ videolan.org  |  Linux: sudo apt install vlc

# 4) Chạy app
python Subtitle_Media_Fetcher_v3.py
```

> Mẹo: đặt `cookies.txt` cạnh script để tăng tỉ lệ tải thành công; app cũng có **fallback** qua `--cookies-from-browser` khi cần.

---

## 🧩 Hướng dẫn sử dụng

### Tab Download
1. **Root**: chọn thư mục gốc để quét `.vtt` (đệ quy).  
2. **Search**: lọc nhanh danh sách.  
3. Chọn một/nhiều `.vtt` → bấm **MP3**, **MP4** hoặc **Both**.  
4. File sẽ được lưu **cạnh phụ đề** và **bỏ qua** nếu đã tồn tại.

### Tab Player
- Dùng chung root hoặc chọn root khác.  
- Tìm & chọn một item (gom theo `[YouTubeID]`).  
- **Play MP3** / **Play MP4**: overlay **song ngữ** (EN trên, VI dưới), đồng bộ theo thời gian.  
- Thiếu media? bấm **Download Missing**.

> VLC chỉ hiển thị một phụ đề mỗi lần; app render **2 dòng** trong UI, nên bạn luôn thấy cả EN & VI.

### Tab Subtitle Search → URL
- Tìm theo tiêu đề hoặc ID; **không trùng lặp theo ID**.
- **Copy URLs** (mục chọn/tất cả) vào clipboard, hoặc **Export** sang file `url_yt.txt`.

---

## ⌨️ Phím tắt (Tab Download)

- **Ctrl+O** – Chọn root  
- **Ctrl+F** – Focus ô tìm kiếm  
- **F5** – Quét lại  
- **Ctrl+1** – Tải MP3  
- **Ctrl+2** – Tải MP4  
- **Ctrl+D** – Tải cả hai  
- **Ctrl+E** – Mở thư mục chứa mục đã chọn  
- **Ctrl+Q** – Thoát

---

## 🛠️ Đóng gói (Build)

Cần `pyinstaller`:
```bash
pip install pyinstaller
```

### Windows (.exe)
```bash
pyinstaller --onefile --windowed --icon nct_logo.ico --add-data "nct_logo.png;." Subtitle_Media_Fetcher_v3.py
# Kết quả: dist/Subtitle_Media_Fetcher_v3.exe
# Mẹo: đặt ffmpeg.exe cạnh .exe nếu máy người dùng chưa có PATH ffmpeg
```

### macOS (.app)
```bash
pyinstaller --onefile --windowed --add-data "nct_logo.png:." Subtitle_Media_Fetcher_v3.py
# Nếu có icon .icns:
# pyinstaller --onefile --windowed --icon nct_logo.icns --add-data "nct_logo.png:." Subtitle_Media_Fetcher_v3.py
# Kết quả: dist/Subtitle_Media_Fetcher_v3.app
```
> Lưu ý: `--add-data` dùng `;` trên Windows, `:` trên macOS/Linux.

---

## 🔒 Xử lý lỗi 403 / Forbidden

App có chuỗi chiến lược giảm lỗi 403:
1. Gọi `yt-dlp` ở Python API với `extractor_args="youtube:player_client=android/web"`  
2. **Retry** với định dạng tổng quát hơn  
3. **Fallback CLI** + `--cookies-from-browser` (Chrome/Edge/Firefox/Brave)  
4. Cuối cùng thử **no-cookies** qua CLI

Bạn cũng có thể đặt `cookies.txt` (định dạng Netscape) cạnh script để tăng lựa chọn.

---

## 🧠 Cách phân tích phụ đề

- Hỗ trợ timestamp `hh:mm:ss.mmm` **và** `mm:ss.mmm`.  
- Parser tối giản, bỏ thẻ HTML cơ bản trong câu phụ đề.  
- Overlay 2 dòng **EN/VI** dựa theo `python-vlc.get_time()`.

---

## 🙋 Khắc phục sự cố

- **Thiếu VLC**: cài VLC runtime và `python-vlc`.  
- **macOS không nhúng video**: một số bản VLC có thể mở cửa sổ riêng; phát vẫn hoạt động.  
- **Tên file Unicode**: được hỗ trợ; nếu chạy từ CLI, đảm bảo terminal hiển thị Unicode.  
- **403 Forbidden**: xem mục xử lý 403; cập nhật yt-dlp (`pip install -U yt-dlp`).  
- **Không thấy ffmpeg**: cài như hướng dẫn hoặc đặt `ffmpeg(.exe)` cạnh app/binary.

---

## 🗺️ Roadmap

- [ ] Thanh tua (seekbar) + tốc độ phát (0.75× / 1.0× / 1.25×)  
- [ ] Export danh sách URL theo từng thư mục  
- [ ] Tuỳ chọn export SRT từ VTT

Rất hoan nghênh đóng góp & PR!

---

## 🧾 Giấy phép

MIT License. (Bạn có thể thay đổi license theo nhu cầu repo.)

---

## ❤️ Lời cảm ơn

- **yt-dlp** cho khả năng trích media mạnh mẽ  
- **VLC / python-vlc** cho phát đa nền tảng  
- **ffmpeg** cho xử lý audio/video

**PharmApp Theme** — by Nghiên Cứu Thuốc / PharmApp.  
Discover • Design • Optimize • Create • Deliver.

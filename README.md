# Fisherfaces: Giảm chiều và Nhận dạng Khuôn mặt (PCA + LDA)

---

## 📖 Tổng quan đồ án

Notebook `LDA.ipynb` cài đặt **thủ công** quy trình toán học của thuật toán **Fisherfaces**, bao gồm:

| Bước | Nội dung |
|------|----------|
| 1 | Nạp và khám phá bộ dữ liệu Olivetti Faces (400 ảnh, 40 người, 64×64 px) |
| 2 | Chia tập Train/Test theo tỉ lệ 70/30 với Stratified Split |
| 3 | Tiền xử lý: Căn giữa dữ liệu (Mean-Centering) |
| 4 | PCA sơ bộ (Eigenfaces) dùng SVD để giải quyết bài toán suy biến |
| 5 | LDA trong không gian con PCA — tính ma trận phân tán S_W, S_B |
| 6 | Kết hợp chiếu Fisherfaces (W_fisher = W_pca × W_lda) |
| 7 | Phân loại Nearest Neighbor và đánh giá mô hình |
| 8 | Trực quan hóa không gian Fisherfaces (2D Scatter Plot) |
| 9 | Kiểm chứng với sklearn.LinearDiscriminantAnalysis |

---

## 🔧 Cài đặt môi trường

### 🪟 Windows

> Thực hiện trong **Command Prompt** (cmd) hoặc **PowerShell**

**Bước 1: Kiểm tra Python**
```cmd
python --version
```
Nếu chưa có, tải Python tại [python.org](https://www.python.org/downloads/) và đảm bảo tick chọn **"Add Python to PATH"** khi cài.

**Bước 2: Clone hoặc tải dự án**
```cmd
git clone <repository-url> lda
cd lda
```

**Bước 3: Tạo môi trường ảo**
```cmd
python -m venv venv
```

**Bước 4: Kích hoạt môi trường ảo**
```cmd
venv\Scripts\activate
```
> Dấu nhắc lệnh sẽ đổi thành `(venv) C:\...`

**Bước 5: Cài đặt các thư viện**
```cmd
pip install numpy matplotlib scikit-learn seaborn notebook
```

**Bước 6: Khởi động Jupyter Notebook**
```cmd
jupyter notebook
```
Trình duyệt sẽ tự mở tại `http://localhost:8888`. Chọn file `LDA.ipynb` để mở.

---

### 🐧 Linux

> Thực hiện trong **Terminal**. Các lệnh dưới áp dụng cho Ubuntu/Debian; distro khác thay `apt` bằng package manager tương ứng (`dnf`, `pacman`, ...).

**Bước 1: Kiểm tra và cài Python**
```bash
python3 --version
# Nếu chưa có:
sudo apt update && sudo apt install python3 python3-pip python3-venv -y
```

**Bước 2: Clone hoặc tải dự án**
```bash
git clone <repository-url> lda
cd lda
```

**Bước 3: Tạo môi trường ảo**
```bash
python3 -m venv venv
```

**Bước 4: Kích hoạt môi trường ảo**
```bash
source venv/bin/activate
```
> Dấu nhắc lệnh sẽ đổi thành `(venv) user@machine:...$`

**Bước 5: Cài đặt các thư viện**
```bash
pip install numpy matplotlib scikit-learn seaborn notebook
```

**Bước 6: Khởi động Jupyter Notebook**
```bash
jupyter notebook
```
Trình duyệt sẽ tự mở tại `http://localhost:8888`. Chọn file `LDA.ipynb` để mở.

---

### 🍎 macOS

> Thực hiện trong **Terminal**

**Bước 1: Kiểm tra Python**
```bash
python3 --version
```
Nếu chưa có, cài đặt qua [Homebrew](https://brew.sh/):
```bash
brew install python
```

**Bước 2: Clone hoặc tải dự án**
```bash
git clone <repository-url> lda
cd lda
```

**Bước 3: Tạo môi trường ảo**
```bash
python3 -m venv venv
```

**Bước 4: Kích hoạt môi trường ảo**
```bash
source venv/bin/activate
```
> Dấu nhắc lệnh sẽ đổi thành `(venv) user@Mac lda %`

**Bước 5: Cài đặt các thư viện**
```bash
pip install numpy matplotlib scikit-learn seaborn notebook
```

**Bước 6: Khởi động Jupyter Notebook**
```bash
jupyter notebook
```
Trình duyệt sẽ tự mở tại `http://localhost:8888`. Chọn file `LDA.ipynb` để mở.

---

## ▶️ Chạy Notebook

Sau khi Jupyter mở trong trình duyệt:

1. Click vào file **`LDA.ipynb`**
2. Chạy toàn bộ notebook theo thứ tự:
   - **Cách 1 (khuyến nghị):** Menu → `Kernel` → `Restart & Run All`
   - **Cách 2:** Chạy từng cell bằng `Shift + Enter`

> ⚠️ **Quan trọng:** Phải chạy các cell **theo đúng thứ tự từ trên xuống** vì các cell phụ thuộc vào biến của cell trước.

> 📝 **Lưu ý:** Lần đầu chạy, scikit-learn sẽ tự động tải bộ dữ liệu Olivetti Faces (~1.4 MB) từ internet và lưu vào thư mục cache. Các lần chạy sau sẽ dùng bản đã tải.

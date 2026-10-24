# Phần 1 – CLI và GUI và cấu trúc câu lệnh

## 1. Cấu trúc câu lệnh

Với lệnh:

```powershell
ls -Force C:\Windows\Logs
```

- **Command:** `ls`
- **Option/Flag:** `-Force`
- **Argument:** `C:\Windows\Logs`

## 2. Vì sao máy chủ Cloud dùng Terminal

Máy chủ chỉ có **TTY**, không có giao diện GUI nên phải dùng Terminal như PowerShell hoặc SSH.

Terminal cũng nhẹ hơn, tốn ít RAM và có thể điều khiển máy chủ từ xa dễ hơn.

---

# Phần 2 – Điều hướng hệ thống file

## 3. Relative Path

Từ:

```text
C:\Users\student\Desktop
```

đi vào thư mục project:

```text
.\quan-ly-sinh-vien
```

Có thể dùng:

```powershell
cd .\quan-ly-sinh-vien
```

## 4. Absolute Path của `main.py`

```text
C:\Users\student\Desktop\quan-ly-sinh-vien\src\main.py
```

## 5. Xem thư mục `data` từ `src`

```powershell
Get-ChildItem ..\data -Force
```

Trong đó `..` có nghĩa là **đi lên thư mục cha**. Từ `src` đi lên project rồi mới vào `data`.

---

# Phần 3 – Thao tác file và thư mục

## 6. Tạo `src` và `data`

```powershell
New-Item -ItemType Directory -Path ".\quan-ly-sinh-vien\src", ".\quan-ly-sinh-vien\data"
```

PowerShell có thể tạo luôn thư mục cha cần thiết.

## 7. Tạo 3 file rỗng

Đang đứng trong thư mục project:

```powershell
New-Item -ItemType File -Path ".\src\main.py", ".\src\utils.py", ".\README.md"
```

## 8. Đổi tên và sao lưu project

Đổi tên thư mục:

```powershell
Rename-Item ".\quan-ly-sv" "quan-ly-sinh-vien"
```

Sao lưu toàn bộ project:

```powershell
Copy-Item ".\quan-ly-sinh-vien" ".\backup-quan-ly-sinh-vien" -Recurse
```

`-Recurse` giúp copy luôn các file và thư mục con.

---

# Phần 4 – Xử lý lỗi Terminal

## 9. Log lỗi số 2

Khả năng cao là **đường dẫn hoặc tên thư mục không tồn tại**, có thể do gõ sai tên hoặc đang đứng sai vị trí.

Có thể gõ vài ký tự đầu rồi nhấn **Tab**. Nếu PowerShell không tự hoàn thành tên thư mục thì nên kiểm tra lại bằng:

```powershell
ls
```

---

## 10. Log lỗi số 3

Lỗi xảy ra vì Windows đang chặn chạy file PowerShell Script do **Execution Policy**.

Có thể sửa bằng:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Sau đó chạy lại script.

---

## 11. Log lỗi số 1

PowerShell cảnh báo vì thư mục `old version` vẫn còn file hoặc thư mục con bên trong.

Xóa toàn bộ mà không hỏi lại:

```powershell
Remove-Item ".\old version" -Recurse -Force
```

- `-Recurse`: xóa cả nội dung con.
- `-Force`: ép thực hiện và hạn chế hỏi xác nhận.

---

## 12. Log lỗi số 4

| Nội dung | Trả lời |
|---|---|
| Dòng lỗi quan trọng | `FileNotFoundError: [Errno 2] No such file or directory` |
| Error Keyword | `FileNotFoundError` |
| Nguyên nhân | Chương trình đang chạy trong `src` nên đường dẫn tới file trong `data` bị sai. |
| Cách sửa | Đi về thư mục gốc project rồi chạy `python .\src\main.py` hoặc dùng đường dẫn `..\data\...` trong chương trình. |

Quy trình em dùng là: **đọc dòng lỗi cuối → tìm từ khóa lỗi → kiểm tra lại đường dẫn/vị trí đang đứng → sửa và chạy lại**.

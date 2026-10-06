# Bài 1: Khảo sát FHS và phân quyền File/Folder nâng cao

## 1. Mục tiêu

Tạo thư mục dùng chung cho ứng dụng web tại `/var/www/my-app`, gồm:

- `public`: chứa các tệp tĩnh công khai.
- `logs`: chứa nhật ký hệ thống cần được bảo vệ.

Tài khoản thực hiện là tài khoản non-root hiện tại (`$USER`), còn nhóm sở hữu là
`www-data` của máy chủ web.

## 2. Nhật ký các lệnh đã thực hiện

> Các lệnh dưới đây được thực hiện trên Linux/Ubuntu. Nếu nhóm `www-data` chưa
> tồn tại trên máy, cần tạo hoặc thay bằng nhóm web tương ứng.

### Tạo cấu trúc thư mục

```bash
sudo mkdir -p /var/www/my-app/public
sudo mkdir -p /var/www/my-app/logs
```

### Gán quyền cho thư mục `public`

Chủ sở hữu có quyền đọc, ghi và truy cập; nhóm có quyền đọc và truy cập;
người dùng khác không có quyền:

```bash
sudo chmod 750 /var/www/my-app/public
```

Quyền `750` tương đương:

```text
Owner: rwx
Group: r-x
Others: ---
```

### Gán quyền cho thư mục `logs`

Chủ sở hữu và nhóm có toàn quyền; người dùng khác không có quyền:

```bash
sudo chmod 770 /var/www/my-app/logs
```

Quyền `770` tương đương:

```text
Owner: rwx
Group: rwx
Others: ---
```

### Gán chủ sở hữu và nhóm sở hữu

```bash
sudo chown -R "$USER":www-data /var/www/my-app
```

Lệnh trên gán tài khoản non-root hiện tại làm chủ sở hữu và `www-data` làm nhóm
sở hữu cho toàn bộ cây thư mục ứng dụng.

## 3. Kiểm tra

```bash
ls -la /var/www/my-app
```

### Kết quả `ls -la /var/www/my-app` copy để nộp bài

Kết quả dưới đây được ghi nhận khi tài khoản hiện tại có tên `student`:

```text
$ ls -la /var/www/my-app
total 16
drwxr-xr-x 4 student www-data 4096 Oct  6 18:00 .
drwxr-xr-x 3 root    root     4096 Oct  6 17:55 ..
drwxrwx--- 2 student www-data 4096 Oct  6 18:00 logs
drwxr-x--- 2 student www-data 4096 Oct  6 18:00 public
```

Trong đó:

- `public` có quyền `drwxr-x---`, tương đương `750`.
- `logs` có quyền `drwxrwx---`, tương đương `770`.
- Cả hai thư mục có chủ sở hữu là tài khoản non-root (`student` trong ví dụ)
  và nhóm sở hữu là `www-data`.
- Ký tự `x` trên thư mục là quyền điều hướng/truy cập vào bên trong thư mục.

Có thể kiểm tra chi tiết hơn bằng lệnh:

```bash
ls -ld /var/www/my-app/public /var/www/my-app/logs
```

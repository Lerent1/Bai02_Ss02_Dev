# Bài 2: Khởi tạo User thường và thiết lập đặc quyền quản trị

## 1. Mục tiêu

* Tạo user thường `devops` trên Ubuntu Server.
* Thêm user `devops` vào nhóm `sudo`.
* Sao chép SSH Key từ root sang user `devops`.
* Thiết lập quyền cho thư mục `.ssh` và file `authorized_keys`.
* Kiểm tra đăng nhập SSH bằng user `devops`.
* Kiểm tra quyền quản trị bằng `sudo whoami`.

---

## 2. Tạo user `devops`

Lệnh đã thực hiện:

```bash
adduser devops
```

Kiểm tra user:

```bash
id devops
```

Kết quả:

```text
uid=1001(devops) gid=1001(devops) groups=1001(devops)
```

---

## 3. Thêm user vào nhóm sudo

Lệnh:

```bash
usermod -aG sudo devops
```

Kiểm tra:

```bash
groups devops
```

Kết quả có nhóm:

```text
sudo
```

---

## 4. Sao chép SSH Key

Sao chép cấu hình SSH từ root sang user `devops`:

```bash
rsync --archive --chown=devops:devops /root/.ssh /home/devops/
```

Kiểm tra:

```bash
ls -la /home/devops/.ssh
```

---

## 5. Thiết lập quyền SSH

Thiết lập quyền cho thư mục `.ssh`:

```bash
chmod 700 /home/devops/.ssh
```

Thiết lập quyền cho file `authorized_keys`:

```bash
chmod 600 /home/devops/.ssh/authorized_keys
```

Đảm bảo user `devops` là owner:

```bash
chown -R devops:devops /home/devops/.ssh
```

Kiểm tra:

```bash
ls -ld /home/devops/.ssh
ls -l /home/devops/.ssh/authorized_keys
```

Kết quả mong đợi:

```text
drwx------ devops devops /home/devops/.ssh
-rw------- devops devops /home/devops/.ssh/authorized_keys
```

---

## 6. Kiểm tra quyền sudo

Chuyển sang user `devops`:

```bash
su - devops
```

Kiểm tra nhóm:

```bash
groups
```

Kết quả có:

```text
sudo
```

Thực hiện:

```bash
sudo whoami
```

Nhập mật khẩu của user `devops`.

Kết quả:

```text
root
```

---

## 7. Kiểm tra đăng nhập SSH

Từ máy cá nhân thực hiện:

```bash
ssh -i /path/to/private_key devops@<IP_ADDRESS_DROPLET>
```

Sau khi đăng nhập thành công:

```bash
whoami
```

Kết quả:

```text
devops
```

Sau đó kiểm tra quyền sudo:

```bash
sudo whoami
```

Kết quả:

```text
root
```

---

## 8. Kết luận

Đã hoàn thành các yêu cầu:

* [x] Tạo user `devops`.
* [x] Thêm `devops` vào nhóm `sudo`.
* [x] Sao chép SSH Key từ root sang `/home/devops/.ssh`.
* [x] Thiết lập quyền `700` cho thư mục `.ssh`.
* [x] Thiết lập quyền `600` cho `authorized_keys`.
* [x] Thiết lập owner `devops:devops`.
* [x] Đăng nhập SSH bằng user `devops`.
* [x] Thực hiện thành công `sudo whoami` với kết quả `root`.

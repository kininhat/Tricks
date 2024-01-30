# MYSQL CMD & Tricks

## I. CMD

### 1. Tạo user

### 2. Grant quyền truy cập

* Từ mysql 5.5 -> 5.7 & mariadb

```bash
GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' IDENTIFIED BY 'mypass';
```

* Mysql 8 - rất đặc biệt khi grant quyền cần phải tạo lại 1 user tương ứng với ip được grant

  * Ví dụ tạo user: root grant quyền cho mọi ip truy cập

```bash
CREATE USER 'root'@'%' IDENTIFIED BY 'root_password';
GRANT ALL PRIVILEGES ON *.* TO 'ub'@'%';
ALTER USER 'ub'@'%' IDENTIFIED WITH caching_sha2_password  BY 'root_password';
FLUSH PRIVILEGES;
```

### 3. Hiển thị các column trong table

```bash
mysql> DESC mytable;
```

## II. Tricks

### 1. Tối ưu database

* Cần backup trước khi thao tác

  * Tối ưu 1 database

    ```bash
    mysqlcheck -o database_name -u root -p
    ```

  * Tối ưu toàn bộ database

    ```bash
    mysqlcheck -o --all-databases  -u root -p
    ```

### 2. Xử lý crash database

# Linux tricks

1. [Check logs](#check-logs)

    1.1. [Webserver](#cl-webserver)

2. [Lỗi vặt](#errors)

    2.1. [Không phân giải được dns](#dns-not-resolve)

## 1. Check logs <a name="check-logs"></a>

### 1.1 Tắt Webserver <a name="cl-webserver"></a>

* Tìm truy cập số  lượng lớn trong khoảng thời gian nhất định, bỏ qua việc đọc erro_log và các file nén .tar.gz

```bash
grep "19:1" * -HRi  | awk '{printf "%s \t %s \t %s \t %s\n", $1, $6, $7, $8}' | sort | uniq -c | sort -hr | head -40 | grep -Ev "*.error.log|*.tar.gz"
```

>> Thay **19:1** thành khoảng thời gian tương ứng

## 2. Lỗi vặt <a name="errors"></a>

### 2.1 Không phân giải được dns <a name="dns-not-resolve"></a>

* Trong quá trình sử dụng có nhiều lúc đặt reverse memory quá cao khiến linux lỗi không phân giải được DNS, khắc phục như sau:

```bash
sysctl -w net.core.rmem_default = 212992
```

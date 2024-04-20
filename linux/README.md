# Linux tricks

1. [Check logs](#check-logs)

    1.1. [Webserver](#cl-webserver)

2. [Lỗi vặt](#errors)

    2.1. [Không phân giải được dns](#dns-not-resolve)

3. [Truy tìm web shell/malware](#find-malware)

    3.1 [Lưu ý và cmd](#notes-cmd)

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

## 3. Truy tìm web shell/malware <a name="find-malware"></a>

* Tìm và hiển thị các file có phần mở rộng .php ở các đường dẫn có tên upload, uploads, css, js, image, asses, images

```bash
find . -type d \( -name upload -o -name uploads -o -name css -o -name js -o -name image -o -name assets -o -name images \) -exec find {} -type f -name "*.php" -ls \;
```

* Tìm các file có permittion không phải 0644

```bash
find . -type f ! -perm 0644 -ls
```

* Tìm các file chứa shell 1 dòng

```bash
for i in `find . -type f -name "*.php" ` ; do if (( `head -1 $i | grep "^<?php" |  wc -m `  > 50 )) ; then echo $i | tee -a shell_1line.txt ; fi  ; done
```

* Tìm các file chứa shell không có khoảng cách

```bash
for i in `find . -type f -name "*\.php"` ; do if [[ `cat $i | grep "^[^[:blank:]/][[:alnum:]]\{5,\}/$" | wc -l ` -gt 10 ]] ; then echo $i | tee -a shell_nospace.txt; fi; done

```

* Tìm các file chứa shell có ký tự đặc biệt

```bash
for i in `find .  -type f -name "*\.php" ` ; do if [[ ` cat $i | grep "\\\\\\\\x[[:digit:]abcdef][[:digit:]abcdef]" -o | wc -l` -gt 50 ]] ; then echo $i | tee -a shell_special_char.txt ; fi; done
```

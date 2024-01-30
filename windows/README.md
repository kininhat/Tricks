# Window tricks

## 1. Powershell

### 1.1 Tắt ArpRetryCount

```bash
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters" /v ArpRetryCount /t REG_QWORD /d 0 /f
```

### 1.2 Chuyển từ window evalution sang bản cao cấp hơn

* win2k19
  * Datacenter:
  
  ```bash
  DISM /Online /Set-Edition:ServerDatacenter /ProductKey:WMDGN-G9PQG-XVVXX-R3X43-63DFG /AcceptEula /y
  ```

  * Standard:
  
  ```bash
  DISM /Online /Set-Edition:ServerStandard /ProductKey:N69G4-B89J2-4G8F4-WWYCC-J464C /AcceptEula /y
  ```

* win2k16
  * Datacenter:
  
  ```bash
    DISM /online /Set-Edition:ServerDatacenter /ProductKey:CB7KF-BWN84-R7R2Y-793K2-8XDDG /AcceptEula /y
  ```

  * Standard:
  
  ```bash
  DISM /Online /Set-Edition:ServerStandard /ProductKey:WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY /AcceptEula /y
  ```

* win2k22
  * Datacenter:
  
  ```bash
    DISM /online /Set-Edition:ServerDatacenter /ProductKey:WX4NM-KYWYW-QJJR4-XV3QB-6VM33 /AcceptEula /y
  ```

  * Standard:
  
  ```bash
  DISM /Online /Set-Edition:ServerStandard /ProductKey:VDYBN-27WPP-V4HQT-9VMD4-VMK7H /AcceptEula /y
  ```

## 2. CMD

### 1.1 renew license 180 ngày

* Chạy cmd

```bash
slmgr.vbs -rearm
```

* Đợi 1 lát và reboot

## 3. Lỗi Remote Desktop bị dính license

* Vào KEY_LOCAL_MACHINE\SYSTEM\ CurrentControlSet\Control\ Terminal Server\RCM xóa mục GracePeriod

* Sau đó restart lại dịch vụ RDP

## 4. Treo game trong vps gpu

### Cách 1

* Tải ultraview, sử dụng ultra để kết nối
* Tạo file out.bat ngoài desktop có nội dung

```bash

@%windir%\System32\tscon.exe 0 /dest:console
@%windir%\System32\tscon.exe 1 /dest:console
@%windir%\System32\tscon.exe 2 /dest:console
@%windir%\System32\tscon.exe 3 /dest:console
@%windir%\System32\tscon.exe 4 /dest:console
@%windir%\System32\tscon.exe 5 /dest:console
```

* Khi out thì ấn file này là được

>> Fix lỗi ultraview như vps gpu bằng cách chạy command này khi thoát:  for /f "skip=1 tokens=3" %%s in ('query user %USERNAME%') do (tscon.exe %%s /dest:console)

### Cách 2

Tạo nhiều user + session remote: <https://github.com/stascorp/rdpwrap>

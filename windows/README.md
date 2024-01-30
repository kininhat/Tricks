# Window tricks

1. [Powershell](#Powershell)

    1.1. [Tắt ArpRetryCount](#offArpRetryCount)

    1.2. [Chuyển từ window evalution sang bản cao cấp hơn](#up-evalution)

    1.2.1. [win2k16](#win2k16)

    1.2.2. [win2k19](#win2k19)

    1.2.3. [win2k22](#win2k22)

2. [CMD](#CMD)

    2.1. [renew license 180 ngày](#renew-win-lic)

3. [Lỗi Remote Desktop bị dính license](#remote-lic)

4. [Treo game trong vps gpu](#treogamegpu)

## 1. Powershell <a name="Powershell"></a>

### 1.1 Tắt ArpRetryCount <a name="offArpRetryCount"></a>

```bash
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters" /v ArpRetryCount /t REG_QWORD /d 0 /f
```

### 1.2 Chuyển từ window evalution sang bản cao cấp hơn <a name="up-evalution"></a>

### 1.2.1 win2k16 <a name="win2k16"></a>

* Datacenter:

```bash
DISM /online /Set-Edition:ServerDatacenter /ProductKey:CB7KF-BWN84-R7R2Y-793K2-8XDDG /AcceptEula /y
```

* Standard:

```bash
DISM /Online /Set-Edition:ServerStandard /ProductKey:WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY /AcceptEula /y
```

### 1.2.2 win2k19 <a name="win2k19"></a>

* Datacenter:

```bash
DISM /Online /Set-Edition:ServerDatacenter /ProductKey:WMDGN-G9PQG-XVVXX-R3X43-63DFG /AcceptEula /y
```

* Standard:
  
```bash
DISM /Online /Set-Edition:ServerStandard /ProductKey:N69G4-B89J2-4G8F4-WWYCC-J464C /AcceptEula /y
```

### 1.2.3 win2k22 <a name="win2k22"></a>
  
* Datacenter:

```bash
DISM /online /Set-Edition:ServerDatacenter /ProductKey:WX4NM-KYWYW-QJJR4-XV3QB-6VM33 /AcceptEula /y
```

* Standard:

```bash
DISM /Online /Set-Edition:ServerStandard /ProductKey:VDYBN-27WPP-V4HQT-9VMD4-VMK7H /AcceptEula /y
```

## 2. CMD <a name="CMD"></a>

### 1.1 renew license 180 ngày CMD <a name="renew-win-lic"></a>

* Chạy cmd

```bash
slmgr.vbs -rearm
```

* Đợi 1 lát và reboot

## 3. Lỗi Remote Desktop bị dính license <a name="remote-lic"></a>

* Vào KEY_LOCAL_MACHINE\SYSTEM\ CurrentControlSet\Control\ Terminal Server\RCM xóa mục GracePeriod

* Sau đó restart lại dịch vụ RDP

## 4. Treo game trong vps gpu <a name="treogamegpu"></a>

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

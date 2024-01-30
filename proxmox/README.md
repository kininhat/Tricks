# Proxmox

## 1. Bật ảo hóa cho virtual machine

* Tại file: /etc/pve/qemu-server/id.conf, sau dòng: agent: 1 điều chỉnh

* Tùy theo dòng cpu mà sẽ có điều chỉnh tương ứng, thường gặp:
  * Basic:

    ```bash
    args: -cpu 'Broadwell-noTSX-IBRS,vmx,hv_time,kvm=off,hv_vendor_id=null,-hypervisor'
    ```

  * Cheap:

    ```bash
    args: -cpu 'IvyBridge-IBRS,vmx,hv_time,kvm=off,hv_vendor_id=null,-hypervisor'
    ```

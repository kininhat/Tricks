# Vmware tricks

## 1. game jx 8 trên vms

```bash
isolation.tools.getPtrLocation.disable = "TRUE"
isolation.tools.setPtrLocation.disable = "TRUE"
isolation.tools.setVersion.disable = "TRUE"
isolation.tools.getVersion.disable = "TRUE"
monitor_control.disable_directexec = "TRUE"
wwn.enabled = TRUE
hypervisor.cpuid.v0 = FALSE
SMBIOS.reflectHost = TRUE
```

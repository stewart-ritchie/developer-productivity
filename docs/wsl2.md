All these changes need a `wsl --shutdown` to take...

Add these changes to `/etc/wsl.conf` to prevent wsl searching Windows path for Linux executables.

```
[interop]
appendWindowsPath=false
```

Confirm path changes...

```bash
echo $PATH | tr ':' '\n'
```

On the Windows host, add these changes to `~\.wslconfig` to limit resources available to wsl (based on 16 processors and 32GB memory on host).

```
[wsl2]
memory=8GB
processors=8
```

On Windows 11, we can also use the newer [mirrored](https://learn.microsoft.com/en-us/windows/wsl/networking#mirrored-mode-networking) networking mode.

```
[wsl2]
networkingMode=mirrored
```

Configure [Hyper-V firewall](https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/hyper-v-firewall) to allow inbound traffic (CAUTION: this is the most flexible option for a development machine. More secure approaches are available for long-term setups).

```powershell
Set-NetFirewallHyperVVMSetting -Name '{40E0AC32-46A5-438A-A0B2-2B479E8F2E90}' -DefaultInboundAction Allow
```

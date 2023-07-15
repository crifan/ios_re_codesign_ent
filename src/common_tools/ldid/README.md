# ldid

* ldid
  * 作用
    * 查看和设置entitlement权限
    * 伪签名
  * 源码
    * saurik
      * git://git.saurik.com/ldid.git
      * https://gitlab.com/opensource-saurik/ldid
    * other fork
      * https://github.com/xerub/ldid

## 基本用法

*  dumps the binary's entitlements
  ```bash
  ldid -e <binary>
  ```
*  sets the binary's entitlements, where ent.xml is the path to an entitlements file
  ```bash
  ldid -Sent.xml <binary>
  ```
* pseudo-signs a binary with no entitlements
  ```bash
  ldid -S <binary>
  ```

## 举例

* if you wanted to add the entitlements in a file on your desktop named Entitlements.xml to debugserver in your current directory, you would run
  ```bash
  ldid -S/Users/you/Desktop/Entitlements.xml ./debugserver
  ```

## 心得

### 二进制有几种架构，ldid就输出几份权限信息

之前已知，查看现有entitlement权限，有2种方式：

* ldid
  ```bash
  ldid -e debugserver
  ```
* codesign
  ```
  codesign -d --entitlements - debugserver
  ```

不过其中有个细节，后来才彻底明白：

* 有时候ldid会输出多份权限信息
  * 原因：二进制中有几个arch，即几种架构，即二进制是FAT，包含多个架构，包含了几个架构，ldid就输出几份权限信息
  * 对比：即使对于FAT的多个架构，`codesign`也只输出1份entitlement权限信息

举例：

```bash
➜  jtool2 pwd
/Users/crifan/dev/dev_tool/reverse_security/iOS/jtool2/jtool2
➜  jtool2 ll
total 10696
-rw-r--r--@ 1 crifan  staff    28K  2 10  2020 WhatsNew.txt
-rwxr-xr-x@ 1 crifan  staff   329K 12 22  2020 disarm
-rwxr-xr-x@ 1 crifan  staff   2.5M  7  3 09:36 jtool2
-rw-r--r--@ 1 crifan  staff   321B  7  3 09:35 jtool2.entitlements
-rwxr-xr-x@ 1 crifan  staff   2.4M  7  3 09:33 jtool2_orig
-rw-r--r--@ 1 crifan  staff    15K 10 22  2022 matchers.txt

➜  jtool2 file jtool2
jtool2: Mach-O universal binary with 3 architectures: [x86_64:Mach-O 64-bit executable x86_64] [arm64:Mach-O 64-bit executable arm64] [arm64e]
jtool2 (for architecture x86_64):    Mach-O 64-bit executable x86_64
jtool2 (for architecture arm64):    Mach-O 64-bit executable arm64
jtool2 (for architecture arm64e):    Mach-O 64-bit executable arm64e
```

即3份架构`x86_64`、`arm64`、`arm64e`的`FAT`多架构的`jtool2`，则此处`ldid`就会输出3份entitlement权限信息：

```bash
➜  jtool2 ldid -e jtool2
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>get-task-allow</key>
    <true/>
    <key>task_for_pid-allow</key>
    <true/>
    <key>run-unsigned-code</key>
    <true/>
</dict>
</plist>
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>get-task-allow</key>
    <true/>
    <key>task_for_pid-allow</key>
    <true/>
    <key>run-unsigned-code</key>
    <true/>
</dict>
</plist>
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>get-task-allow</key>
    <true/>
    <key>task_for_pid-allow</key>
    <true/>
    <key>run-unsigned-code</key>
    <true/>
</dict>
</plist>
```

与之对比，codesign则始终只输出1份entitlement权限信息：

```bash
➜  jtool2 codesign -d --entitlements - jtool2
Executable=/Users/crifan/dev/dev_tool/reverse_security/iOS/jtool2/jtool2/jtool2
[Dict]
    [Key] get-task-allow
    [Value]
        [Bool] true
    [Key] run-unsigned-code
    [Value]
        [Bool] true
    [Key] task_for_pid-allow
    [Value]
        [Bool] true
```

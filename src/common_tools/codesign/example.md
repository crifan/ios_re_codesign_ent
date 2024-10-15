# 举例

## jtool2_orig

```bash
➜  jtool2 codesign -vv -d jtool2_orig
Executable=/Users/crifan/dev/dev_tool/reverse_security/iOS/jtool2/jtool2/jtool2_orig
Identifier=jtool2.arm64e
Format=Mach-O universal (x86_64 arm64e arm64)
CodeDirectory v=20001 size=6446 flags=0x0(none) hashes=193+5 location=embedded
jtool2_orig: no signature
Info.plist=not bound
TeamIdentifier=not set
Sealed Resources=none
Internal requirements count=0 size=12
```

## debugserver

```bash
crifan@licrifandeMacBook-Pro  ~/dev/dev_root/iosReverse/AppleStore/dynamicDebug/debugserver_lldb  codesign -d --entitlements - debugserver
Executable=/Users/crifan/dev/dev_root/iosReverse/AppleStore/dynamicDebug/debugserver_lldb/debugserver
debugserver: no signature
��qqJ<?xml version="1.0" encoding="UTF-8"?>
...
```
* 说明
  * `no signature`：表示没有代码签名

## Preferences

### Preferences.app

```bash
➜  Preferences.app pwd
/Users/crifan/dev/dev_root/iosReverse/AppleStore/Preferences_app/ipa/Payload/Preferences.app
➜  Preferences.app cd ..
➜  Payload ll
total 0
drwxr-xr-x@ 50 crifan  staff   1.6K  3  7 14:46 Preferences.app
➜  Payload codesign -vv -d Preferences.app
Executable=/Users/crifan/dev/dev_root/iosReverse/AppleStore/Preferences_app/ipa/Payload/Preferences.app/Preferences
Identifier=com.apple.Preferences
Format=app bundle with Mach-O thin (arm64)
CodeDirectory v=20400 size=1326 flags=0x2(adhoc) hashes=31+7 location=embedded
Signature=adhoc
Info.plist entries=39
TeamIdentifier=not set
Sealed Resources version=2 rules=9 files=1
Internal requirements count=0 size=12
```

### Preferences.app/Preferences

```bash
➜  Payload codesign -vv -d Preferences.app/Preferences
Executable=/Users/crifan/dev/dev_root/iosReverse/AppleStore/Preferences_app/ipa/Payload/Preferences.app/Preferences
Identifier=com.apple.Preferences
Format=app bundle with Mach-O thin (arm64)
CodeDirectory v=20400 size=1326 flags=0x2(adhoc) hashes=31+7 location=embedded
Signature=adhoc
Info.plist entries=39
TeamIdentifier=not set
Sealed Resources version=2 rules=9 files=1
Internal requirements count=0 size=12
```

## Apple Store.app

```bash
➜  425AB24D-6133-4247-AF3A-7D7AE10B3FFC pwd
/Users/crifan/dev/dev_root/iosReverse/AppleStore/fromiPhone11/AppleStore_official_inited/1st/Bundle/425AB24D-6133-4247-AF3A-7D7AE10B3FFC
➜  425AB24D-6133-4247-AF3A-7D7AE10B3FFC ll
total 16
drwxr-xr-x@ 70 crifan  staff   2.2K  1 31 10:54 Apple Store.app
-rw-r--r--@  1 crifan  staff   654B  1 31 10:55 BundleMetadata.plist
-rw-r--r--@  1 crifan  staff   1.4K  1 31 10:55 iTunesMetadata.plist

➜  425AB24D-6133-4247-AF3A-7D7AE10B3FFC codesign -vv -d Apple\ Store.app
Executable=/Users/crifan/dev/dev_root/iosReverse/AppleStore/fromiPhone11/AppleStore_official_inited/1st/Bundle/425AB24D-6133-4247-AF3A-7D7AE10B3FFC/Apple Store.app/Apple Store
Identifier=com.apple.store.Jolly
Format=app bundle with Mach-O thin (arm64)
CodeDirectory v=20500 size=2596 flags=0x0(none) hashes=35+7 location=embedded
Signature size=4390
Authority=Apple iPhone OS Application Signing
Authority=Apple iPhone Certification Authority
Authority=Apple Root CA
Info.plist entries=59
TeamIdentifier=APPLECOMPUTER
Sealed Resources version=2 rules=33 files=2422
Internal requirements count=1 size=104
```

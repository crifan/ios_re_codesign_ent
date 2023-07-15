# 运行崩溃killed

iOS（或Mac中）的app，由于签名方面问题，运行时出现崩溃问题，且提示Killed，可能的情况有多种：

## debugserver运行崩溃killed

* 现象：用`ldid`重签名后的debugserver在`iOS 15.0`的`iPhone8`上运行出现崩溃提示killed
  ```bash
  iPhone8-150:/usr/bin root# /usr/bin/debugserver --version
  zsh: killed     /usr/bin/debugserver --version
  ```
* 原因：代码签名方面的问题
  * 查看崩溃日志ips，可以看到是代码签名问题
    * `type EXC_CRASH signal SIGKILL CODESIGNING`
  * 对比
    * `iOS 15`之前可以用`ldid`重签名 -> 运行不会崩溃
    * 但`iOS 15`之后会崩溃
      * iOS 15之后，应该是内部做了签名方面的校验和判断
* 解决办法：要改用`codesign`重签名
  ```bash
  codesign -f -s - --entitlements debugable_entitlement.xml debugserver
  ```
  * 注：核心差异应该是，`-s -`会用上当前的签名

由此有了心得：

### ldid换codesign

* iOS 15之前
  ```bash
  ldid -Sdebugserver.entitlements debugserver
  ```
    * 可以正常运行
* iOS 15之后
  ```bash
  ldid -Sdebugserver.entitlements debugserver
  ```
    * 运行崩溃：`Killed 9``
      * 解决办法：换`codesign`
          ```bash
          codesign -f -s - --entitlements debugserver.entitlements debugserver
          ```

即：

* 给debugserver添加entitlement权限（后，再放回iPhone中使用）
  * `< iOS 15`：可以用`ldid`
    ```bash
    ldid -Sdebugable_entitlement.xml debugserver
    ```
  * `>= iOS 15`
    * 不能用ldid
      * 否则就会运行报错killed
        * 查看崩溃日志ips，可以看到是：代码签名问题
          * `type EXC_CRASH signal SIGKILL CODESIGNING`
    * 要用`codesign`（估计是其中带了`signature`）
      ```bash
      codesign -f -s - --entitlements debugable_entitlement.xml debugserver
      ```


## Mac M2 Max中jtool2运行崩溃Killed

* 现象

brew安装的官网的jtool2

```bash
brew install --cask jtool2
```

运行时报错：`Unrecoverable CT signature issue, bailing out`

```bash
默认    17:41:55.432539+0800    kernel    Checking in with amfid for DER jtool2.arm64e
默认    17:41:55.433342+0800    kernel    AMFI: /Users/crifan/dev/dev_tool/reverse_security/iOS/jtool2/jtool2_old/jtool2 doesn't have DER entitlements and will not work in a future release
默认    17:41:55.433439+0800    kernel    AMFI: '/Users/crifan/dev/dev_tool/reverse_security/iOS/jtool2/jtool2_old/jtool2' has no CMS blob?
默认    17:41:55.433454+0800    kernel    AMFI: '/Users/crifan/dev/dev_tool/reverse_security/iOS/jtool2/jtool2_old/jtool2': Unrecoverable CT signature issue, bailing out.
默认    17:41:55.433480+0800    kernel    AMFI: Releasing transmuted blob for jtool2.arm64e - <private> 94x
默认    17:41:55.433516+0800    kernel    proc 77535: load code signature error 4 for file "jtool2"
默认    17:41:55.434075+0800    kernel    exec_mach_imgact: not running binary "jtool2" built against preview arm64e ABI
默认    17:41:55.434268+0800    kernel    ASP: Security policy would not allow process: 77535, /Users/crifan/dev/dev_tool/reverse_security/iOS/jtool2/jtool2_old/jtool2
```

* 原因：签名方面问题
* 解决过程

用codesign去签名

```bash
codesign -f -s - --entitlements jtool2.entitlements jtool2
```

虽然能解决：`Unrecoverable CT signature issue, bailing out`的签名问题，会仍会出现其他签名方面问题：

`unsuitable CT policy 0 for this platform/device, rejecting signature.`
  ```bash
  默认    09:46:52.624806+0800    kernel    AMFI: '/Users/crifan/dev/dev_tool/reverse_security/iOS/jtool2/jtool2/jtool2' is adhoc signed.
  默认    09:46:52.624815+0800    kernel    AMFI: '/Users/crifan/dev/dev_tool/reverse_security/iOS/jtool2/jtool2/jtool2': unsuitable CT policy 0 for this platform/device, rejecting signature.
  默认    09:46:52.624854+0800    kernel    proc 13558: load code signature error 4 for file "jtool2"
  默认    09:46:52.625370+0800    kernel    exec_mach_imgact: not running binary "jtool2" built against preview arm64e ABI
  默认    09:46:52.625580+0800    kernel    ASP: Security policy would not allow process: 13558, /Users/crifan/dev/dev_tool/reverse_security/iOS/jtool2/jtool2/jtool2
  ```

-> 最终解决办法：

* 不用brew安装官网的jtool2 = 卸载掉原先安装的版本
  ```bash
  brew uninstall jtool2
  ```
* 换装其他版本的jtool2
  ```bash
  brew install --no-quarantine excitedplus1s/repo/jtool2
  ```

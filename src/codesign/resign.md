# 重新签名

* `重新代码签名=重签名`
  * 何时需要去重签名
    * app的代码签名已过期
    * 想要更改app的权限
      * 比如
        * 加上可被调试相关entitlement权限后，重新签名打包，使其可调试
* 如何重新签名
  * 常用工具：`codesign`、`ldid`
  * 具体命令+举例
    * `codesign`
      ```bash
      codesign -f -s - --entitlements debugserver.entitlements debugserver
      ```
      * 参数说明
        * `-f`==`--force`：`强制`覆盖已有签名
        * `-s -`
          * `-s`==`--sign`
          * `-`：=`single letter`=`dash`，表示`ad-hoc signing`
        * `debugserver.entitlements`：（经修改后加了进程可调试的的）新的权限文件
        * `debugserver`：目标文件=现有的二进制文件
    * `ldid`
      ```bash
      ldid -Sdebugserver.entitlements debugserver
      ```
      * 参数说明
        * `-Sdebugserver.entitlements` = `-S`不带空格的加上`newEntitlementFile`
        * `debugserver`：目标文件=现有的二进制文件

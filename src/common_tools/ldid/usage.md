# 基本用法

* ldid基本用法
  * 查看/导出二进制的entitlement权限 = dumps the binary's entitlements
    ```bash
    ldid -e <binary>
    ```
  * 设置二进制的entitlement权限 = sets the binary's entitlements, where ent.xml is the path to an entitlements file
    ```bash
    ldid -Sent.xml <binary>
    ```
    * 注：`-S`和文件名`ent.xml`中间**没有空格**！
  * 伪签名二进制，无entitlement权限 = pseudo-signs a binary with no entitlements
    ```bash
    ldid -S <binary>
    ```

## 举例

* 给debugserver重新签名
  ```bash
  ldid -Sdebugserver.entitlements debugserver
  ```
  * 参数说明
    * `-Sdebugserver.entitlements` = `-S`不带空格的加上`newEntitlementFile`
    * `debugserver`：目标文件=现有的二进制文件

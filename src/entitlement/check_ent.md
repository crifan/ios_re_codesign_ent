# 查看权限

* 查看权限
  * 常用工具：`codesign`、`ldid`
  * 具体命令
    * `codesign`
      * 二进制文件
        ```bash
        codesign -d --entitlements - BinaryFile
        ```
        * 举例
          ```bash
          codesign -d --entitlements - debugserver
          codesign -d --entitlements - amsaccountsd_orig
          ```
      * app
        ```bash
        codesign -d --entitlements - xxx.app
        ```
        * 举例
          ```bash
          codesign -d --entitlements - Preferences.app
          ```
    * `ldid`
      ```bash
      ldid -e BinaryFile
      ```
      * 举例
        ```bash
        ldid -e debugserver
        ldid -e amsaccountsd
        ldid -e Preferences.app/Preferences
        ```

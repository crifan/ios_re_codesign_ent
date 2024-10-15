# codesign

* codesign
  * 用法
    * 查看签名
      ```bash
      codesign -vv -d iOSBinaryFile
      codesign -vv -d xxx.App
      ```
    * 重新签名
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

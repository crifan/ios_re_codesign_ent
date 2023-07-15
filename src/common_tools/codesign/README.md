# codesign

## 心得

### security+codesign -> codesign

* 之前用：`security`+`codesign ...`
  ```bash
  security find-identity -p codesigning
  codesign --force --sign CDDC8C0D2F3A79EB17C183E14F799F75815E294E --entitlements debugable_entitlement.xml --timestamp=none --generate-entitlement-der debugserver_resign
  ```
* 现在改用：`codesign ... -s - ...`
  ```bash
  codesign --force --sign - --entitlements debugable_entitlement.xml --timestamp=none --generate-entitlement-der debugserver_resign
  ```
  * 区别：`-`=`短横线`：表示`ad-hoc signing`，无需再去用`security`去寻找当前可用签名了
  * 可简写为
    ```bash
      codesign -f -s - --entitlements debugserver.entitlements debugserver_resign
    ```

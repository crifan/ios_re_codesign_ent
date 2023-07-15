# network相关

之前是遇到：

给debugserver重签名之前，要删除原entitlement中的：

* `com.apple.security.network.server`和`com.apple.security.network.client`
  * 目的：防止后续lldb调试报错Failed to get connection from a remote gdb process

## 官网解释

* [com.apple.security.network.server](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_security_network_server?language=objc)
  * A Boolean value indicating whether your app may listen for incoming network connections.
* [com.apple.security.network.client](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_security_network_client?language=objc)
  * A Boolean value indicating whether your app may open outgoing network connections.

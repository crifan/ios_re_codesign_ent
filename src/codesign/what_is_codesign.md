# 什么是codesign代码签名

iOS的app，做了很多安全防护，其中之一就是：`codesign`=`代码签名`

加了代码签名，可以防止未经授权的app程序运行。只有通过签名校验的app才能正常运行。

增强了iOS系统的安全性，对应的，增加了iOS逆向的难度。

* app内部就包含`Codesignature`签名信息
  * ![ios_app_arch](../assets/img/ios_app_arch.png)

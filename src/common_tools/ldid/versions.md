# 不同版本

* **brew**（安装的Cellar）版本：
  * 位置：`/usr/local/bin/ldid`
    * 其是软链接
      * 实际位置是：`/usr/local/Cellar/ldid/2.1.5/bin/ldid`
  * 大小：`814K`
  * 文件格式：`Mach-O 64-bit executable x86_64`
* **iOSOpenDev**版本
  * 位置：`/opt/iOSOpenDev/bin/ldid`
  * 大小：`383K`
  * 文件格式：
    * `(for architecture i386):    Mach-O executable i386`
    * `(for architecture x86_64):    Mach-O 64-bit executable x86_64`

-> 结论：

* 优先用**brew版本**的`ldid`
  * 尽量不要用iOSOpenDev的ldid

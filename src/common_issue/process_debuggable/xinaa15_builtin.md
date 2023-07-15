# XinaA15自带支持所有进程可调试

XinaA15开启越狱期间，有个日志提示：

```bash
签名debugserver
```
  * ![xinaa15_log_resign_debugserver](../../assets/img/xinaa15_log_resign_debugserver.png)

估计就是指的是：

* 给debugserver重签名，使得其支持：所有进程可调试

得到我们要的效果：

XinaA15越狱后，任何进程均可调试。

## 举例

### `设置`=`Preferences`

iOS内置app：`设置`=`Preferences`

* 已注入
  * ![xinaa15_process_preferences](../../assets/img/xinaa15_process_preferences.png)
    * 进程PID是`2354`

Xcode中去通过PID`2354`去调试Preferences：

![xcode_debug_pid_2354](../../assets/img/xcode_debug_pid_2354.png)

即可顺利调试Preferences：

![xcode_debug_preferences_bt](../../assets/img/xcode_debug_preferences_bt.png)

### `debugserver`

`XinaA15`中的`debugserver`的进程：

* debugserver：显示已被`(注入)(P)`
  * ![xinaa15_process_debugserver_p](../../assets/img/xinaa15_process_debugserver_p.png)
* 点击弹框可以查看到
  * 已注入了库：`xxx/procursus/usr/lib/TweakInject.dylib`
    * ![xinaa15_debugserver_tweakinject_dylib](../../assets/img/xinaa15_debugserver_tweakinject_dylib.png)
* 去查看`ENT`=`entitlement`=`权限`，发现已经有我们希望的值了
  * `task_for_pid-allow`
    * ![xinaa15_debugserver_task_for_pid_allow](../../assets/img/xinaa15_debugserver_task_for_pid_allow.png)
  * `get-task-allow`
    * ![xinaa15_debugserver_get_task_allow](../../assets/img/xinaa15_debugserver_get_task_allow.png)

### `akd`

`XinaA15`中的`akd`的进程：

* 去查看`ENT`=`entitlement`=`权限`，发现已经有我们希望的值了
  * `task_for_pid-allow` + `proc_info-allow`
    * ![xinaa15_akd_task_for_pid_allow](../../assets/img/xinaa15_akd_task_for_pid_allow.png)
  * `get-task-allow`
    * ![xinaa15_akd_get_task_allow](../../assets/img/xinaa15_akd_get_task_allow.png)
  * `platform-application`
    * ![xinaa15_akd_platform_application](../../assets/img/xinaa15_akd_platform_application.png)

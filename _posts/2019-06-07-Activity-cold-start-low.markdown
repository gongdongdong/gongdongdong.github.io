## 冷启动的学习

## Activitymanagerservice 探索

![时序图](http://assets.processon.com/chart_image/5cfa8a6fe4b0a64c88abf0f4.png)

修改了/home/monomania/WORKING_DIRECTORY/frameworks/base/services下面的部分类，

通过 mmm frameworks/base/services/ 编译

再通过adb sync 和我的nexus 5x 同步，

检查日志：

通过冷启动命令启动

```
06-08 00:56:29.334 31677 31777 E gdd     : activitystarter -> startActivityMayWait1  ===  1559926589334
06-08 00:56:29.334 31677 31777 E gdd     : activitystarter -> startActivityMayWait2  ===  1559926589334
06-08 00:56:29.335 31677 31777 E gdd     : activitystarter -> startActivityMayWait4  ===  1559926589335
06-08 00:56:29.335 31677 31777 E gdd     : activitystarter -> startActivityMayWait5  ===  1559926589335
06-08 00:56:29.335 31677 31777 I ActivityStarter: START u0 {flg=0x10000000 cmp=com.example.android.architecture.blueprints.todomvp.mock/com.example.android.architecture.blueprints.todoapp.tasks.TasksActivity} from uid 0
06-08 00:56:29.354 31677 31777 E gdd     : activitystarter -> startActivityMayWait6  ===  1559926589354
06-08 00:56:29.354 31677 31777 E gdd     : activitystarter -> startActivityMayWait7  ===  1559926589354
06-08 00:56:29.898 31677 31703 I ActivityRecord: Displayed com.example.android.architecture.blueprints.todomvp.mock/com.example.android.architecture.blueprints.todoapp.tasks.TasksActivity: +530ms
06-08 00:56:29.899 31677 31777 E gdd     : activitystarter -> startActivityMayWait8  ===  1559926589899
06-08 00:56:29.899 31677 31777 E gdd     : activitymetricslogger -> notifyActivityLaunched  ===  1559926589899

```

通过launcher 启动

```
06-08 00:57:06.567 31677 32406 E gdd     : activitymangerservice -> startActivity1  ===  1559926626567
06-08 00:57:06.567 31677 32406 E gdd     : activitymangerservice -> startActivityAsUser  ===  1559926626567
06-08 00:57:06.567 31677 32406 E gdd     : activitystarter -> startActivityMayWait1  ===  1559926626567
06-08 00:57:06.567 31677 32406 E gdd     : activitystarter -> startActivityMayWait2  ===  1559926626567
06-08 00:57:06.567 31677 32406 E gdd     : activitystarter -> startActivityMayWait4  ===  1559926626567
06-08 00:57:06.568 31677 32406 E gdd     : activitystarter -> startActivityMayWait5  ===  1559926626568
06-08 00:57:06.568 31677 32406 I ActivityStarter: START u0 {act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=com.example.android.architecture.blueprints.todomvp.mock/com.example.android.architecture.blueprints.todoapp.tasks.TasksActivity bnds=[437,1635][643,1913]} from uid 10014
06-08 00:57:06.582 31677 32406 E gdd     : activitystarter -> startActivityMayWait6  ===  1559926626582
06-08 00:57:06.582 31677 32406 E gdd     : activitystarter -> startActivityMayWait7  ===  1559926626582
06-08 00:57:06.582 31677 32406 E gdd     : activitystarter -> startActivityMayWait8  ===  1559926626582
06-08 00:57:06.582 31677 32406 E gdd     : activitymetricslogger -> notifyActivityLaunched  ===  1559926626582
06-08 00:57:07.088 31677 31703 I ActivityRecord: Displayed com.example.android.architecture.blueprints.todomvp.mock/com.example.android.architecture.blueprints.todoapp.tasks.TasksActivity: +496ms

```


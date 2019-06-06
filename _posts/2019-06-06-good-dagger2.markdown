## DAGGER2 的一种非常妙的用法



> 需要添加Gradle 依赖库
>
> ```
> // Dagger dependencies
> def daggerVersion = '2.22.1'
> annotationProcessor "com.google.dagger:dagger-compiler:$daggerVersion"
> implementation "com.google.dagger:dagger:$daggerVersion"
> implementation "com.google.dagger:dagger-android:$daggerVersion"
> implementation "com.google.dagger:dagger-android-support:$daggerVersion"
> annotationProcessor "com.google.dagger:dagger-android-processor:$daggerVersion"
> ```



## dagger2 的一般用法

定义需要使用注入的接口

```
@Component(modules = OneModule.class)
public interface MainComponent {
    void inject(MainActivity activity);
}
```

可以使用module 和 provider 注解来提供注入的方法

```
@Module
public class OneModule {

    public OneModule(){
    }
    @Provides
    public List getList(){
        List one = new LinkedList();
        one.add("33333");
        return one;
    }
}
```

在需要注入的activity中需要用一句话进行注入

```
注入需要调用的方法
DaggerMainComponent.builder().build().inject(this);
---------
注入的表达式：
@Inject
List one;
```





## 现在有一种非常妙的用法，是google提供的。

------------

google提供了  DaggerApplication  DaggerAppCompatActivity 来一起见证奇迹吧

------

需要一个Application 继承 DaggerApplication

```
public class MyApp extends DaggerApplication {
    @Override
    protected AndroidInjector<? extends DaggerApplication> applicationInjector() {
        return DaggerAppComponent.builder().application(this).build();
    }
}
```

需要一个applicationComponent

```
@Singleton
@Component(modules = {ActivityBindingModule.class,
        AndroidSupportInjectionModule.class})
public interface AppComponent extends AndroidInjector<MyApp> {

    // Gives us syntactic sugar. we can then do DaggerAppComponent.builder().application(this).build().inject(this);
    // never having to instantiate any modules or say which module we are passing the application to.
    // Application will just be provided into our app graph now.
    @Component.Builder
    interface Builder {

        @BindsInstance
        AppComponent.Builder application(Application application);

        AppComponent build();
    }
}
```

一个绑定Activity的module

```
@Module
public abstract class ActivityBindingModule {
    @ContributesAndroidInjector(modules = ForDaggerModule.class)
    abstract ForDaggerActivity myForDaggerActivity();
}
```

一个Activity 继承 DaggerAppCompatActivity

```
public class ForDaggerActivity extends DaggerAppCompatActivity {

    @Inject
    MyContainer myContainer;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.show_dagger_layout);
        Log.e("tag", myContainer.toString());
    }
}
```

和平常的用法相比，我们在Activity中的DaggerMainComponent.builder().build().inject(this);就不需要了，Google已经在DaggerAppCompatActivity中操作过了。



是不是看上去比一般用法简洁不少。
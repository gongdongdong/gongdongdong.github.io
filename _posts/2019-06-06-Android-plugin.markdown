## android 某种类型的 plugin的书写

1. 创建一个java project

2. 删除其中的java ， 创建一个 groovy目录 里面创建一个StandAlonePlugin.groovy

   ```
   package com.learn.plugin
   
   import org.gradle.api.Plugin
   import org.gradle.api.Project
   
   /**
    * Created by GongDongdong on 2019/5/25.
    */
   
   class StandAlonePlugin implements Plugin<Project> {
       void apply(Project project) {
           note()
           //create an extension object:Whyn,so others can config via Whyn
           project.extensions.create("whyn", YNExtension)
           project.task('whyn'){
               group = "test"
               description = "gradle Standalone project demo,shares everywhere"
               doLast{
                   println '**************************************'
                   println "$project.whyn.description"
                   println '**************************************'
               }
           }
       }
   
       private void note(){
           println '------------------------'
           println 'apply StandAlonePlugin'
           println '------------------------'
       }
   }
   
   class YNExtension {
       String description = 'default description'
   }
   ```

3. 创建一个resources目录，里面创建一个META-INF文件夹创建一个gradle-plugins文件夹 创建一个com.learn.plugin.StandAlonePlugin.properties文件，里面就一句话  

   ``` implementation-class=com.learn.plugin.StandAlonePlugin ```

4. build.gradle 中的内容如下

   ```
   apply plugin: 'groovy'
   
   dependencies {
       //gradle sdk
       implementation gradleApi()
       //groovy sdk
       implementation localGroovy()
   }
   
   
   apply plugin: 'maven-publish'
   
   publishing {
       publications {
           mavenJava(MavenPublication) {
   
               groupId 'com.learn.plugin'
               artifactId 'ynplugin'
               version '1.0.0'
   
               from components.java
   
           }
       }
   }
   
   publishing {
       repositories {
           maven {
               // change to point to your repo, e.g. http://my.org/repo
               url uri('/home/monomania/Android/repos')
           }
       }
   }
   ```

5. gradle sync之后，可以看到我们的gradle中有publishing 里面有publish

我们可以publish我们的插件了 ， 会在下面的目录中有我们的插件

```            url uri('/home/monomania/Android/repos') ```

这种类型的插件的写法如上了。


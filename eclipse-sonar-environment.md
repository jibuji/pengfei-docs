Eclipse + Sonar环境
背景：

我们平时本地编译apk是这么做的：
  `./apk xxx.mk`
  
如果加上`-s`,像这样:
    `./apk -s xxx.mk`
    
在编译完成之后，就可以在下面的任何一个链接里看到：
`http://18.8.10.91/sonar/`，
`http://ss.alm.gionee.com/sonar/`，
`http://19.9.0.104:9000/sonar/`，
这三个链接都会映射到同一个服务器，所以内容是一样的。

我们可以在这些web页面中查看具体的代码问题点，但没办法直接对代码修改，于是，我们需要切换到eclipse中，找到对应的代码，然后修改，这个过程效率很低。

目的：
解决修改问题代码效率低的问题。最终效果是这样的：在eclipse中可以看到问题点，点击问题点，可以自动跳转到问题代码处，即可直接修改。如图![Cool! Right?](./final-demo.jpg)
原理：
![sonar architecture](technical-architecture.jpg)

但其实本方案并非完美，有如下不足：
  1. 在eclipse-sonar插件中分析的结果与我们平时采用`./apk -s xxx.mk`方法相比，分析的结果不同。
  2. 当我们连接`http://18.8.10.91/sonar/`等现有的sonar服务器时，eclipse中在分析的时候会报错，所以我自己建了一个sonar服务器，连接公司服务器失败的同事，可以连接我们自己的。
  
  
步骤：
1. 下载sonar插件：
Eclipse下选择Help > Install New Software。在选择Add按钮后，增加sonar插件的更新地址http://dist.sonar-ide.codehaus.org/eclipse/。 选择组件SonarQube Java。完成安装。
2. 配置：
在eclispse的Window->Preference的SonarQube项 添加sonar服务器地址如图
3. 在eclipse中右键点击项目，选择Configure->Associate with SonarQube，如图。在SonarQube Project这一列，填入你要进行代码检查的项目名，当输入前三个字符时，他会自动联想出剩下的内容，如果找不到项目，在左上方会有提示，此时就需要在build_apk_env中执行-s命令了。执行后在进行此操作就OK了。
4. 现在就可以再次右键点击项目，就会有一个SonarQube菜单项，如图选择Analyze即可开始检查代码, 这需要一段时间，请耐心等待
5. 检查完成后，会出现这样的界面，如图。在SonarQube Issues视图里可以看到代码问题点，展开子项，双击即可跳转到问题代码处。如果没有看到SonarQube Issues视图，可以在Window->show views中找到。


# Maven项目父子版本统一管理&amp;引用parent无法使用占位符版本号

`Maven`项目中一般都会使用占位符语法来统一引用的三方包版本号。

但是，如果对当前父子项目统一管理使，会出现如下问题。下面是子项目引用父项目出现的问题。

![image_10.png](image_10.png)

意思大概就是引用父级版本最好是实际的版本号，而不是占位符，这玩意儿吧，不报错。`Maven`编译也能通过，就是恶心。一直寻思怎么去掉他。

终于得到了答案，分享出来。

使用`Maven`的`revision+flatten-maven-plugin`来解决问题


## revision
首先在父项目的`properties`标签里加入`revision`标签，里面指定版本。

```Shell
<properties>
    <revision>1.0.0</revision>
</properties>
```

然后，在引用父项目的时候，`<verson>`里写的是`${revision}`

```Shell
<parent>
    <groupId>xxx</groupId>
    <artifactId>xxx</artifactId>
    <version>${revision}</version>
</parent>
```

## flatten-maven-plugin
接着添加`flatten-maven-plugin`插件

```Shell
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>flatten-maven-plugin</artifactId>
    <version>1.5.0</version>
    <configuration>
        <updatePomFile>true</updatePomFile>
        <flattenMode>oss</flattenMode>
    </configuration>
    <executions>
        <execution>
            <id>flatten</id>
            <goals>
                <goal>flatten</goal>
            </goals>
            <phase>process-resources</phase>
        </execution>
        <execution>
            <id>flatten.clean</id>
            <goals>
                <goal>clean</goal>
            </goals>
            <phase>clean</phase>
        </execution>
    </executions>
</plugin>
```

PS：插件一定要在父项目中加，而且子项目的`<version>`标签就可以删除了。否则，子项目依赖其他子项目的时候，可能会出错。


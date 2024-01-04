# Java多态与Lombok的关系-1

最近在重新整理支付项目代码，要做的抽象一点儿，毕竟之前被某位大哥整了一个支付类4k+代码。想用多态，结果瞎比，全忘了，重新回忆一下。



父类：

```Java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Father implements Serializable {

    public String name;

}
```

子类：

```Java
@Data
@NoArgsConstructor
@ToString(callSuper = true)
@AllArgsConstructor
public class Son extends Father implements Serializable {

    private String nickName;


    @Builder(builderMethodName = "childBuilder", toBuilder = true)
    public Son(String name, String nickName) {
        super(name);
        this.nickName = nickName;
    }
}
```



测试类：

```Java
public class Test {

    public static void main(String[] args) {
        test(get());
    }

    public static void test(Father father) {
        Son son = (Son) father;
        System.out.println(JSON.toJSONString(son));
    }


    public static Father get() {
        Son son = Son.childBuilder().name("son").nickName("sonNick").build();
        return son;
    }
}
```



上面的代码意思是调用`get`方法，虽然返回是父类。但是`get`方法里面最终返回的是子类。

调用`test`方法，入参虽然是父类，但是传入的是子类。子类强转父类是可以转的。

然后`lombok`贼拉好用，但是，`lombok`的父类与子类的处理比较麻烦。

需要在子类自定义`builder`，因为，如果父类有`@Builder`的话，子类不能有`@Builder`注解，否则会报错的。但是，子类的`builder`方法是拿不到父类的字段的。需要在子类写个构造方法，上面加上注解`@Builder(builderMethodName = "childBuilder", toBuilder = true)`



`builderMethodName`是方法名，你起什么方法名，子类的`build`方法就是什么名。

`toBuilder`是决定子类能否在`build`方法里引用父类字段的，且父类字段必须是`public`不能是`private`的，否则还是调用不到。
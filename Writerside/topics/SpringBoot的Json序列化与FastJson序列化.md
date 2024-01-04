# SpringBoot的Json序列化与FastJson序列化

最近在跟前端联调的时候，发现个问题，`Java`后端返回给前端的参数，如果是`Long`的话，前端接收的时候，会出现精度丢失的情况。

前端让返回`String`。由于项目是使用的`fastjson`，于是用了`fastjson`的`@JSONField(serializeUsing = ToStringSerializer.class)`注解。

发现不起作用。查阅资料后才知道。`SpringBoot`的`Json`序列化默认是`Jackson`。于是，就把`SpringBoot`的默认序列化给修改了，代码如下：


```Java
@Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        converters.add(new ByteArrayHttpMessageConverter());
        converters.add(new StringHttpMessageConverter(StandardCharsets.UTF_8));// @ResponseBody 解决乱码
        converters.add(new ResourceHttpMessageConverter());
        converters.add(new AllEncompassingFormHttpMessageConverter());
        converters.add(fastJsonHttpMessageConverter());
    }

    @Bean
    public FastJsonHttpMessageConverter fastJsonHttpMessageConverter() {
        FastJsonHttpMessageConverter converter = new FastJsonHttpMessageConverter();
        FastJsonConfig config = new FastJsonConfig();
        //Long类型转String类型
//        SerializeConfig serializeConfig = SerializeConfig.globalInstance;
        // ToStringSerializer 是这个包 com.alibaba.fastjson.serializer.ToStringSerializer
//        serializeConfig.put(BigInteger.class, ToStringSerializer.instance);
//        serializeConfig.put(Long.class, ToStringSerializer.instance);
//        serializeConfig.put(Long.TYPE, ToStringSerializer.instance);
//        config.setSerializeConfig(serializeConfig);
        config.setSerializerFeatures(
                SerializerFeature.WriteMapNullValue, // 保留map空的字段
                SerializerFeature.WriteNullStringAsEmpty, // 将String类型的null转成""
                SerializerFeature.WriteNullNumberAsZero, // 将Number类型的null转成0
                SerializerFeature.WriteNullListAsEmpty, // 将List类型的null转成[]
                SerializerFeature.WriteNullBooleanAsFalse, // 将Boolean类型的null转成false
                SerializerFeature.WriteDateUseDateFormat,  //日期格式转换
                SerializerFeature.DisableCircularReferenceDetect // 避免循环引用
        );
        config.setSerializeFilters(valueFilter);
        converter.setFastJsonConfig(config);
        converter.setDefaultCharset(StandardCharsets.UTF_8);
        // 解决中文乱码问题，相当于在Controller上的@RequestMapping中加了个属性produces = "application/json"
        List<MediaType> mediaTypeList = new ArrayList<>();
        mediaTypeList.add(MediaType.APPLICATION_JSON);
        converter.setSupportedMediaTypes(mediaTypeList);
        return converter;
    }

    /**
     * FastJson过滤器将null值转换为字符串
     * obj 是class
     * s 是key值
     * o1 是value值
     */
    public static final ValueFilter valueFilter = (obj, s, v) -> {
        if (v == null) {
            return "";
        }
        return v;
    };
```

上面的代码有部分注释掉的情况，那个如果不注释掉，所有接口返回的`Long`类型都会变成`String`。到这儿，本来为问题解决了。可谁知道前几天突然有个同事说支付不好使了。于是就去看啥问题。我返回的字段是这样的`aLiPayStr`。前端同学接收的是`aliPayStr`。于是，主观意识告诉我，前端同学错了。我就告诉他们别乱改接收返回字段，前端同学也很委屈，我没有改，`git`提交记录显示代码压根没动过。

于是，我就犯嘀咕了，接口出参没动过，我的`git`提交记录也显示好久没修改了，前端也是，那么到底哪出了问题呢。而且就测试环境有问题，别的环境好好的，我就去仔细过了一下我的代码。发现有上面代码的合并记录。翻阅相关资料，发现了`Jackson`的一个奇葩的地方，也是个坑，告诉大家，不要踩坑。`SpringBoot`的默认`Json`序列化`Jackson`在对参数返回的时候，会对字段属性头两个字母做小写处理。。。

我惊呆了，你这玩意儿写的咋这么反人类，因为我把默认`Json`序列化给修改成阿里的`fastjson`。于是问题就出现了，阿里的`fastjson`是你的字段属性是啥，人家就给你返回啥，而不是把你的字段属性的前两个字母给变成小写。以前返回的一直是`aliPayStr`，换成`fastjson`后，就变成了跟我字段属性写的一样`aLiPayStr`。

于是，代码回滚。但是，代码回滚的话`Long`转`String`咋整。

嗯，用了`Jackson`的`@JsonSerialize(using = ToStringSerializer.class)`注解就解决了问题。

其实，写到这里，大家可能觉得无所谓，但是吧，我们平时开发总是会犯一个主观性的错误，就是我用了啥，返回的就是啥，其实不是。用什么框架，不能说原理全部了解，但是，一些小细节还是需要知道的。不然就会向我一样犯错误。

嗯，写完了，拜拜~
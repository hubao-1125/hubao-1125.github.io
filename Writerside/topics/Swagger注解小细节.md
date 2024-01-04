# Swagger注解小细节

最近，在忙公司的一个新的项目，全新的，从头整的。在写接口文档的时候，偶然发现的`Swagger`的一个小问题。分享给大家，希望大家在用的时候，能够注意一下，话不多说，上报错。

```
Errors
Hide
 
Resolver error at paths./acc/startStop.post.parameters.0.schema.$ref
Could not resolve reference: Could not resolve pointer: /definitions/启用/禁用cmd does not exist in document
```

就是某个接口的入参文档不生成，只要启动项目访问`Swagger`就会有报错，但是不影响其他文档的。
这个问题咋发现的呢，前端同学说你这个啥也没有哇，我就去看了，然后仔细排查，发现了问题。

`Swagger`的`@ApiOperation`注解，在使用的时候，我们会在两个括号里面写`value = ""`，这个双引号里面就是接口名称，意思是这个接口是做啥的。

`Swagger`解析不了`/`这个符号，于是，就无法生成对应这个接口的入参，对，没错，只影响入参，出参么得问题。就很奇怪。

嗯，拜拜~
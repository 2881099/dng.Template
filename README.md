模板引擎，不再使用（作纪念）

# 安装

> Install-Package dng.Template -Version 1.0.0

# 使用方法

```csharp
//定义静态对象
static dng.Template tpl = new dng.Template("/views"); //监视目录，文件变化会重新加载编译

//渲染
tpl.RenderFile("123.html", 渲染数据); //渲染数据为字典类型
```

# 模板语法

```html
<html>
<head>
<title>{#title}</title>
</head>
<body>

<!--绑定表达式-->
{#表达式}
{##表达式} 当表达式可能发生runtime错误时使用，性能没有上面的高

<!--可嵌套使用，同一标签最多支持3个指令-->
{include ../header.html}
<div @for="i 1, 101">
  <p @if="i === 50" @for="item,index in data">aaa</p>
  <p @else="i % 3 === 0">bbb {#i}</p>
  <p @else="">ccc {#i}</p>
</div>

<!--定义模块，可以将公共模块定义到一个文件中-->
{module module_name1 parms1, 2, 3...}
{/module}
{module module_name2 parms1, 2, 3...}
{/module}

<!--使用模块-->
{import ../module.html as myname}
{#myname.module_name(parms1, 2, 3...)}

<!--继承-->
{extends ../inc/layout.html}
{block body}{/block}

<!--嵌入代码块-->
{%
for (var a = 0; a < 100; a++)
  print(a);
%}

<!--条件分支-->
{if i === 50}
{elseif i > 60}
{else}
{/if}

<!--三种循环-->
{for i 1,101}                可自定义名 {for index2 表达式1 in 表达式2}

{for item,index in items}    可选参数称 index
                             可自定义名 {for item2, index99 in 数组表达式}

{for key,item,index on json} 可选参数 item, index，
                             可自定义名 {for key2, item2, index99 in 对象表达式}
{/for}

<!--不被解析-->
{miss}
此块内容不被bmw.js解析
{/miss}

</body>
</html>
```
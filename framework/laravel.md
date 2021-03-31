## 使用模板继承

先在/resources/views/admin下新建一个布局模板 base.blade.php

> 注意 下面代码不是最新版,请与你下载的版本来写,这里只是示例

```
~~~

<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <title>{{$title}} - {{config('web.web_name')}}</title>
    <meta name="csrf-token" content="{{csrf_token()}}">
    <link rel="stylesheet" href="{{asset('/static/common/layui/css/layui.css')}}?v={{$version}}">
    <link rel="stylesheet" href="{{asset('/static/admin/css/style.css')}}?v={{$version}}">
    <script src="{{asset('/static/common/layui/layui.js')}}?v={{$version}}"></script>
    <script src="{{asset('/static/common/js/jquery-3.3.1.min.js')}}?v={{$version}}"></script>
    <script src="{{asset('/static/common/js/vue.min.js')}}?v={{$version}}"></script>
    <script src="{{asset('/static/common/js/axios.min.js')}}?v={{$version}}"></script>
    @yield('style')
</head>
<body>

<div id="app">
    <!--顶栏-->
    <header>
        <h1 v-text="webname"></h1>
        <div class="breadcrumb">
            <i class="layui-icon">&#xe715;</i>
            <ul>
                <li v-for="vo in address">
                    <a  v-text="vo.name" :href="vo.url" ></a> <span>/</span>
                </li>
            </ul>
        </div>
    </header>

    <div class="main">
        <!--左栏-->
        <div class="left">
            <ul class="cl" >
                <!--顶级分类-->
                <li v-for="vo,index in menu" :class="{hidden:vo.hidden}">
                    <a href="javascript:;" :class="{active:vo.active}" @click="onActive(index)">
                        <i class="layui-icon" v-html="vo.icon"></i>
                        <span v-text="vo.name"></span>
                        <i class="layui-icon arrow" v-show="vo.url.length==0">&#xe61a;</i> <i v-show="vo.active" class="layui-icon active">&#xe623;</i>
                    </a>
                    <!--子级分类-->
                    <div v-for="vo2,index2 in vo.list">
                        <a href="javascript:;" :class="{active:vo2.active}" @click="onActive(index,index2)" v-text="vo2.name"></a>
                        <i v-show="vo2.active" class="layui-icon active">&#xe623;</i>
                    </div>
                </li>
            </ul>
        </div>
        <!--右侧-->
        <div class="right">
            <!--你的内容这里面-->
            @yield('body')

        </div>
    </div>
</div>
<script type="text/javascript">
    var webname = '{{config("web.web_name")}}';
    var menuUrl = '/static/data/menu.json';
</script>
<script src="{{asset('/static/admin/js/script.js')}}?v={{$version}}"></script>

@yield('script')

</body>
</html>

~~~

```
然后新建后面首页模板 /resources/views/admin/index.blade.php


```
~~~

<!--引入base-->
@extends('admin.base')

<!--独立页面的css,用不到可以删除-->
@section('style')
    <style>
        .right h2{
            margin: 10px 0;
        }
        .right li{
            margin-bottom: 10px;
        }
    </style>
@endsection

<!--独立页面的js,用不到可以删除-->
@section('script')
    <script>
        //...
    </script>
@endsection

<!--这里是你的页面内容-->
@section('body')
    <blockquote class="layui-elem-quote">
        <h2>QAdmin 轻量级后台模板</h2>
        <p>基于layui框架与Vue.js构建</p>
        <p>轻量不复杂 简洁不简单</p>
    </blockquote>
    .......

@endsection


~~~
```

然而你以后新建的页面,只需引用以下代码即可


```

<!--引入base-->
@extends('admin.base')

<!--独立页面的css,用不到可以删除-->
@section('style')
    <style>
        .right h2{
            margin: 10px 0;
        }
        .right li{
            margin-bottom: 10px;
        }
    </style>
@endsection

<!--独立页面的js,用不到可以删除-->
@section('script')
    <script>
        //...
    </script>
@endsection

<!--这里是你的页面内容-->
@section('body')

```




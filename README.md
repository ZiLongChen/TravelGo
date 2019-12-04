# travelgo

> 如下命令来打包生成一个vuedd项目vue init webpack travelgo

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

一:知识点

1：vue中链接
<!--在vue中链接可以使用router-link-->
<router-link to="/list" class="home">列表页</router-link>

2：单页应用
页面跳转---->是通过JS渲染的   即通过路由   
优点：页面切换快   不需要去请求html，那么减少了请求时间    缺点：seo差  百度等搜索引擎不会将其排名靠前

3：项目初始化配置
  1).index.html的meta中配置
<!--由于是移动端使用  minimum-scale=1.0,maximum-scale=1.0,user-scalable=no 
      此举禁止用户通过手指放大缩小  比例始终为1：1-->
  2).引入reset.css
  目的：手机浏览器的限制   在入口文件main.js引入此css保证页面初始化样式一致
  3).引入border.css
  目的：手机大屏可能导致的1像素问题  即在大屏显示中  1像素可能显示多像素
  4).引入fastClick库
   目的：由于有些浏览器的限制  click事件可能延迟几秒  导致用户体验感差
   操作：npm install fastclick --save  将此库安装在项目的依赖中   --save是不管在开发环境还是生产环境都要使用fastclick
   使用：
        main.js:import fastClick from 'fastclick'   
                fastClick.attach(document.body)这样子就可以使用fastClick了
  5).此时用了阿里巴巴的iconfont创建了图标管理项目
  6).在项目中安装stylus   
  目的：便于css样式代码编写
  操作:npm install stylus --save   npm install stylus-loader --save

4:项目中stylus的使用
  1).<style lang="stylus" scoped></style>即可使用stylus   scoped限制只在当前组件使用  不会影响其他组件
  2).1rem = html的font-size=50px   也就是43px等于0.86rem---->  .86rem

5:iconfont的使用
  1).到iconfont将需要的图标添加至购物车 下载   将.eot,.svg,.ttf,.woff结尾的新建文件夹iconfont放置其中
  2).将iconfont.css放置styles目录
  3).在iconfont.css中将里面的url改成对应上述四个后缀结尾的文件所在位置
  4).图标的使用  <span class="iconfont">&#xe624;</span>命名一个iconfont的类，在iconfont的网站复制对应图标的代码
6:全场主色调 
  1).可以创建一个varibles.styl文件代表这是一个stylus要使用的全局变量文件
  在对应要使用主色调的css中添加 @import '~@/assets/styles/varibles.styl'  此时的@符号代表src目录  当在css样式文件中引入其他css时  要在@前添加~
  2).在varibles.styl中定义主色调的变量在需要使用该颜色的文件内引用  变量名可随意起
7:别名配置
  1).在文件中 经常引用到src目录下的styles目录下的文件
     那么可以在webpack.base.conf.js文件下增加如下所示的styles别名去代替完整的路径
     resolve: {
      extensions: ['.js', '.vue', '.json'],
      alias: {
        'vue$': 'vue/dist/vue.esm.js',
        '@': resolve('src'),
        'styles': resolve('src/assets/styles')
      }
    }
  2).修改了配置项之后要重启服务

8:轮播图使用了GitHub上搜索的surmon-china/vue-awesome-swiper
  1):优点能够简单的建立一个轮播
  2):使用(可参考github上的手册)
    便于稳定使用2.6.7版本
    1.npm install vue-awesome-swiper@2.6.7 --save
    2.在main.js上引用
    import VueAwesomeSwiper from 'vue-awesome-swiper'
    import 'swiper/dist/css/swiper.css'
    3.穿透  在wrapper下的这个样式穿透变红色
    .wrapper >>> .swiper-pagination-bullet-active
        background :red !important
9:三个点省略号(在对应文字上样式)
    overflow: hidden
    white-space: nowrap   不进行换行
    text-overflow: ellipsis   以省略号代表被修剪的文字
10:可以复用样式  利用stylus提供的styl去定义一个方法样式
    mixins.styl
      ellipsisxxx()
      overflow: hidden
      white-space: nowrap
      text-overflow: ellipsis
    使用：
    css中导入@import '~styles/mixins.styl';
    在需要的地方添加对应的ellipsisxxx()

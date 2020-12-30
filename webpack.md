## webpack
>+ webpack4.x 版本后拆分了webpack和webpack-cli
>+ webpack依赖于node.尽量将node升级为最新版本,因为node越稳定,webpack打包速度越快
>+ npm install时默认安装的是生产依赖

### 两种启动方式
>+ npx webpack ---- 通过npx命令找到nodemodules中的webpack  [npx是npm5.x携带的功能,通过npx命令只会在当前目录下的node_modules中.bin的目录中的软连接下去查找],, **npm webpack 不可以**
>+ npm run dev  ---- 通过在package.json中配置scripts的方式启动

### 打包模式mode,不同的打包模式会启用不同的打包插件
>+ production  会将代码进行压缩  默认的mode就是生产模式
>+ development 不压缩代码,可以再开发中提高开发的效率,准确的给出代码错误提示信息,并且给chunk和module命名
>+ none 不指定 不启用任何优化的插件,chunk命名为0,1,2,..

### 配置
默认配置文件名是webpack.config.js.  4.0以后,webpack有零配置的功能,项目中可以不写任何配置文件就可对项目进行打包[默认打包js文件],但是其内置功能特别弱,所以还是需要开发者去配置

>+ entry : 入口文件,默认的入口路径是 './src/index.js';
>+ output : 打包输入位置 ,
output : {
    path: path.resolve(__dirname, './dist')   ----注意需要是绝对路径,一般使用node的path模块实现
    filename:  'main.js'  ---输出文件名
}
>+ mode : 打包模式
>+ bundle : 资源经过webpack打包流程解析编译后最终输出的成果文件,一个bundle包含一个chunk,一个chunk可能有多个chunk模块
>+ chunk : 指的是代码块,一个chunk可能由多个模块组合而成,也用于代码的合并与分割
>+ loader : 默认情况下,webpack仅支持.js文件,通过loader.可以让他解析其他类型的文件.坚持一个loader只做一件事情的原则
>+ plugin : plugin的作用是让webpack认识更多的不同文件类型,而plugin的职责则是让其控制构建流程,从而执行一些特殊的任务.
>>+ 作用于webpack打包整个过程
>>+ webpack的打包过程是由(声明周期概念)钩子的


bundle,chunk,module三者的关系
index.js -> a.js -> bundle[chunks: index.js a.js]
bundle文件的内容: [webpack启动函数,chunks(可能是一个module,可能是多个module)]

### 打包后的文件分析
打包后的文件[***即bundle***]就是一个个的webpackBootstrap(webapck启动函数),,其实就是自执行函数(function(){}()),自执行函数的参数是个对象,里面包含了一个又一个的key文件路径,值是一个函数的对象

### 常用loader举例分析  -- 严格遵守,一个loader只做一件事情
css-loader  => webpack支持.css模块的语法
style-loader  => 把css的代码抽离出来,动态的生成style标签,并将css代码放入style标签中
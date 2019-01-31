#### 前言

vue脚手架的3.x版本已经在开发中，现在还处于alpha版本。以下是上手记录，方便以后查找。

#### 开始
##### 1、环境安装
```
npm install -g @vue/cli
```
##### 2、创建命令

```
vue create <project-name>
```
##### 3、配置环境变量

```
VUE_APP_API_ROOT = 'https://XXX'
VUE_APP_CURRENTMODE = 'production'
```
采坑：相较于vue-cli2.x,可能有采用如下配置方式，来区分开发环境、测试环境和正式环境的环境变量：

```
// 开发环境 development
// config/dev.env.js
module.exports = merge(prodEnv, {
  NODE_ENV: '"development"',
  API_ROOT: '"localhost:xxxx"'，
  ...
}）
// 测试环境 test
// config/test.env.js
module.exports = merge(prodEnv, {
  NODE_ENV: '"test"',
  API_ROOT: '"https://test.example.net"',
  ...
}）
// 正式环境 production
// config/dev.env.js
module.exports = merge(prodEnv, {
  NODE_ENV: '"test"',
  API_ROOT: '"https://example.net"',
  ...
}）
```

打包时，运行不同的打包命令即可.

但是，再vue-cli3.0中，官方脚手架只支持开发环境和正式环境，要实现测试环境打包，则需要借助模式mode的概念，将打包命令稍作修改：

```
"scripts": {
    "dev": "vue-cli-service serve",
    "test": "vue-cli-service build --mode test",
    "build": "vue-cli-service build"  
  }
```

配置环境变量：

```
// 开发环境 development
// .env.development
NODE_ENV = 'development'
VUE_APP_CURRENTMODE = 'development'
VUE_APP_API_ROOT = 'localhost:xxxx'
...

// 测试环境 test
// .env.test
NODE_ENV = production
VUE_APP_CURRENTMODE = 'test'
VUE_APP_API_ROOT = 'https://test.example.net'
...

// 正式环境 production
// .env.production
NODE_ENV = production
VUE_APP_CURRENTMODE = 'production'
VUE_APP_API_ROOT = 'https://example.net'
...
```



##### 4、配置vue.config.json

```
/**
 *	vue.config.js 配置说明
 *  这里只列一部分，具体配置惨考文档啊
 */

module.exports = {
  baseUrl: process.env.NODE_ENV === 'production' ? '/online/' : '/',

  /* outputDir: 在npm run build时 生成文件的目录 type:string, default:'dist' */
  outputDir: 'dist',

  /* lintOnSave：{ type:Boolean default:true } 问你是否使用eslint */
  lintOnSave: true,

  /**
   * productionSourceMap：{ type:Bollean,default:true } 生产源映射
   * 如果您不需要生产时的源映射，那么将此设置为false可以加速生产构建 
   */
  productionSourceMap: false,

  /**
   * devServer:{type:Object} 3个属性host,port,https 
   * 它支持webPack-dev-server的所有选项 
   */

  devServer: {
    port: 8085,
    host: 'localhost',
    https: false, // https:{type:Boolean}
    open: true, //配置自动启动浏览器
    // proxy: 'http://localhost:4000' // 配置跨域处理,只有一个代理
    proxy: {
      '/api': {
        target: '<url>',
        ws: true,
        changeOrigin: true
      },
      '/foo': {
        target: '<other_url>'
      }
    },  // 配置多个代理
  },

  /** 
   * @vue/cli  内部的webpack 配置是通过 webpack-chain 维护的
   * 此处以实际项目中用到的svg（基于svg-sprite-loader）配置为例
   */

  chainWebpack: config => {
    // 一个规则里的 基础Loader
    // svg是个基础loader
    const svgRule = config.module.rule('svg')

    // 清除已有的所有 loader。
    // 如果你不这样做，接下来的 loader 会附加在该规则现有的 loader 之后。
    svgRule.uses.clear()

    // 添加要替换的 loader
    svgRule
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({
        symbolId: 'icon-[name]'
      })
  }
}
```
##### 5、配置tsconfig.json

```
{
  "compilerOptions": {
    // 解析非相对模块名的基准目录
    "baseUrl": ".",
    // 编译输出目标 ES 版本（es5）
    "target": "esnext",
    // 采用的模块系统
    "module": "esnext",
    // 以严格模式解析
    "strict": true,
    "jsx": "preserve",
    // 从 tslib 导入外部帮助库: 比如__extends，__rest等
    "importHelpers": true,
    // 如何处理模块
    "moduleResolution": "node",
    // 启用装饰器
    "experimentalDecorators": true,
    "esModuleInterop": true,
    // 启用设计类型元数据（用于反射）
    "emitDecoratorMetadata": true,
    // 在表达式和声明上有隐含的any类型时报错
    "noImplicitAny": false,
    // 不是函数的所有返回路径都有返回值时报错。
    "noImplicitReturns": true,
    // 允许从没有设置默认导出的模块中默认导入
    "allowSyntheticDefaultImports": true,
    // 是否包含可以用于 debug 的 sourceMap
    "sourceMap": true,
    // 将每个文件作为单独的模块
    "isolatedModules": false,
    // 编译过程中打印文件名
    "listFiles": true,
    // 移除注释
    "removeComments": true,
    // 允许编译javascript文件
    "allowJs": true,
    "types": [
      "node",
      "mocha",
      "chai"
    ],
    // 指定特殊模块的路径
    "paths": {
      "@/*": [
        "src/*"
      ]
    },
    // 编译过程中需要引入的库文件的列表
    "lib": [
      "esnext",
      "dom",
      "dom.iterable",
      "scripthost"
    ]
  },
  // 编译过程中需要编译的文件
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "tests/**/*.ts",
    "tests/**/*.tsx"
  ],
  // 编译过程中需要排除编译的文件
  "exclude": [
    "node_modules"
  ]
}
```
##### 结束语

持续关注中，如有错误，欢迎批评指正~
	
	
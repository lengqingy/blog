# 什么是单元测试

维基百科：单元测试是针对 程序的最小单元 来进行正确性检验的测试工作。程序单元是应用的最小可测试部件。一个单元可能是单个程序、类、对象、方法等。

# 单元测试的意义

减少bug、提高代码质量、快速定位bug、减少调试时间、放心重构。

# 单元测试的目的

当你的项目足够大的时候，在叠加模块和组件的过程中，是很有可能影响之前的模块。

但是被影响的模块已经通过了测试，我们在迭代的时候，很少有测试人员会去重新测试这个系统。所以， 被影响的模块很可能就有了一个隐形的bug被部署到线上。

因此我们采用自动化测试。最主要的作用是对于大型项目，在每次迭代的时候， 可以保证整个系统的正确运行， 确保系统的健壮。

另外值得注意的是，单元测试并不能完全代替功能测试，因为程序本身设计的逻辑错误或者其它的一些环境因素所造成的影响，单元测试可能无能为力。

所以，单元测试只是保证你想让程序模块输出一只猪，它不会整出一头驴来。至于进一步的功能测试或者说“肉测”，仍然是有必要的。

# 成熟好用的测试工具库 -- vue-test-utils

[vue-test-utils](https://vue-test-utils.vuejs.org/zh/) 是 Vue 生态圈中的一个开源项目，其前身是 avoriaz，avoriaz 也是一个不错的包，但其 README 中有说明，当 vue-test-utils 正式发布的时候， 它将会被废弃。

vue-test-utils 能极大地简化 Vue.js 单元测试。

例如，网上一搜 Vue 单元测试，得到的例子一般是像下面这样的（包括 vue-cli 提供的模板里默认也是这样）：

    import Vue from 'vue'
    import HelloWorld from '@/components/HelloWorld'
    
    describe('HelloWorld.vue', () => {
      it('should render correct contents', () => {
        const Constructor = Vue.extend(HelloWorld)
        const vm = new Constructor().$mount()
        expect(vm.$el.querySelector('.hello h1').textContent)
          .toEqual('Welcome to Your Vue.js App')
      })
    })
    
使用 vue-test-utils 后，你可以像下面这样

    import { shallow } from '@vue/test-utils'
    import HelloWorld from '@/components/HelloWorld'
    
    describe('HelloWorld.vue', () => {
      it('should render correct contents', () => {
        const wrapper = shallow(HelloWorld, {
          attachToDocument: ture
        })
    
        expect(wrapper.find('.hello h1').text()).to.equal('Welcome to Your Vue.js App')
      })
    })

可以看到代码更加简洁了。wrapper 内含许多有用的方法，上面的例子中所使用的 find() 其中最简单不过的一个。vue-test-utils 还有 createLocalVue() 等方法以及 stub 之类的功能，基本上可以完成绝大部分情况下的测试用例。

# 选择一个测试运行器

测试运行器 (test runner) 就是运行测试的程序。

主流的 JavaScript 测试运行器有很多，但 Vue Test Utils 都能够支持。它是与测试运行器无关的

当然在我们选用测试运行器的时候也需要考虑一些事项：功能集合、性能和对单文件组件预编译的支持等。在仔细比对现有的库之后，我们推荐其中的两个测试运行器：

- [Jest](https://jestjs.io/docs/zh-Hans/getting-started) 是功能最全的测试运行器。它所需的配置是最少的，默认安装了 JSDOM，内置断言且命令行的用户体验非常好。不过你需要一个能够将单文件组件导入到测试中的预处理器。我们已经创建了 vue-jest 预处理器来处理最常见的单文件组件特性，但仍不是 vue-loader 100% 的功能。
- [mocha-webpack](https://github.com/zinserjan/mocha-webpack) 是一个 webpack + Mocha 的包裹器，同时包含了更顺畅的接口和侦听模式。这些设置的好处在于我们能够通过 webpack + vue-loader 得到完整的单文件组件支持，但这本身是需要很多配置的。

### 浏览器环境

Vue Test Utils 依赖浏览器环境。技术上讲你可以将其运行在一个真实的浏览器，但是我们并不推荐，因为在不同的平台上都启动真实的浏览器是很复杂的。我们推荐取而代之的是用 JSDOM 在 Node 虚拟浏览器环境运行测试。

### 测试单文件组件

Vue 的单文件组件在它们运行于 Node 或浏览器之前是需要预编译的。我们推荐两种方式完成编译：通过一个 Jest 预编译器，或直接使用 webpack。

`vue-jest` 预处理器支持基本的单文件组件功能，但是目前还不能处理样式块和自定义块，这些都只在 `vue-loader` 中支持。如果你依赖这些功能或其它 webpack 特有的配置项，那么你需要基于 webpack + `vue-loader` 进行设置。

因此这里有不同的设置方式:
- 用 Jest 测试单文件组件
- 用 Mocha 和 webpack 测试单文件组件

# Jest 相对于 karma + mocha + Chrome 组合的优缺点

## 优点

1、一站式的解决方案

在使用 Jest 之前，我需要一个测试框架（mocha），需要一个测试运行器（karma），需要一个断言库（chai），需要一个用来做 spies/stubs/mocks 的工具（sinon 以及 sinon-chai 插件），一个用于测试的浏览器环境（可以是 Chrome 浏览器，也可以用 PhantomJS）。

而使用 Jest 后，只要安装它，全都搞定了。

2、全面的官方文档，易于学习和使用

est 的官方文档很完善，对着文档很快就能上手。而在之前，我需要学习好几个插件的用法，至少得知道 mocha 用处和原理吧 我得学会 karma 的配置和命令，chai 的各种断言方法……，经常得周旋于不同的文档站之间，其实是件很烦也很低效的事。

3、配置简单方便

4、更直观明确的测试信息提示

5、方便的命令行工具

全局安装 Jest 后，可以在命令行执行单元测试，配合各种命令参数，可以方便地实现执行单个测试、监视文件变化并自动执行等功能。特别是对于监视文件变化并执行，它提供多种模式，可以只执行修改过的测试。

## 缺点

1、jsdom 的一些局限性

因为 Jest 是基于 jsdom 的，jsdom 毕竟不是真实的浏览器环境，它在测试过程中其实并不真正的“渲染”组件。这会导致一些问题，例如，如果组件代码中有一些根据实际渲染后的属性值进行计算（比如元素的 clientWidth）就可能出问题，因为 jsdom 中这些参数通常默认是 0.

所以有些情况下，测试中可能要施以一些骚操作，比如自行 mock（实例上就是伪造，但合理地伪造）一些中间值，来满足测试用例。如果你的项目中这样的情况很多，还是建议使用 karma + mocha + chrome 这一组合。

2、周边相关的包可能还不完善

例如 vue-jest，目前的版本并不能完全实现 vue-loader 的功能。比如，使用 sass，postcss 之类的功能，它会抛出警告信息。代码中直接 import 实际的 css 文件，则有可能报错，这时则需要使用 mock 来模拟 css 文件。这些问题，在使用 karma-mocha Chrome 的时候是没有的，因为测试运行于真实的浏览器环境中。


# 用 Jest 测试单文件组件

jest 是一个由 Facebook 开发的测试运行器，致力于提供一个“bettery-included”单元测试解决方案。你可以在其[官方文档](https://jestjs.io/zh-Hans)学习到更多 Jest 的知识。

### 安装 Jest

我们假定你在一开始已经安装并配置好了 webpack、vue-loader 和 Babel——例如通过 vue-cli 创建了 webpack-simple 模板脚手架。

我们要做的第一件事就是安装 Jest 和 Vue Test Utils：

    $ npm install --save-dev jest @vue/test-utils
    
然后我们需要在 package.json 中定义一个单元测试的脚本。

    // package.json
    {
      "scripts": {
        "test": "jest"
      }
    }

### 在 Jest 中处理单文件组件

为了告诉 Jest 如何处理 *.vue 文件，我们需要安装和配置 vue-jest 预处理器：

    npm install --save-dev vue-jest
    
接下来在 package.json 中创建一个 jest 块：

    {
      // ...
      "jest": {
        "moduleFileExtensions": [
          "js",
          "json",
          // 告诉 Jest 处理 `*.vue` 文件
          "vue"
        ],
        "transform": {
          // 用 `vue-jest` 处理 `*.vue` 文件
          ".*\\.(vue)$": "vue-jest"
        }
      }
    }
    
### 处理 webpack 别名

如果你在 webpack 中配置了别名解析，比如把 `@` 设置为 `/src` 的别名，那么你也需要用 `moduleNameMapper` 选项为 Jest 增加一个匹配配置：

    {
      // ...
      "jest": {
        // ...
        // 支持源代码中相同的 `@` -> `src` 别名
        "moduleNameMapper": {
          "^@/(.*)$": "<rootDir>/src/$1"
        }
      }
    }
    
### 为 Jest 配置 Babel

尽管最新版本的 Node 已经支持绝大多数的 ES2015 特性，你可能仍然想要在你的测试中使用 ES modules 语法和 stage-x 的特性。为此我们需要安装 babel-jest：

    npm install --save-dev babel-jest
    
接下来，我们需要在 `package.json` 的 `jest.transform` 里添加一个入口，来告诉 Jest 用 `babel-jest` 处理 JavaScript 测试文件：

    {
      // ...
      "jest": {
        // ...
        "transform": {
          // ...
          // 用 `babel-jest` 处理 js
          "^.+\\.js$": "<rootDir>/node_modules/babel-jest"
        }
        // ...
      }
    }
    
> 默认情况下，babel-jest 会在其安装完毕后自动进行配置。尽管如此，因为我们已经显性的添加了对 *.vue 文件的转换，所以现在我们也需要显性的配置 babel-jest。

我们假设 webpack 使用了 babel-preset-env，这时默认的 Babel 配置会关闭 ES modules 的转译，因为 webpack 已经可以处理 ES modules 了。然而，我们还是需要为我们的测试而开启它，因为 Jest 的测试用例会直接运行在 Node 上。

同样的，我们可以告诉 babel-preset-env 面向我们使用的 Node 版本。这样做会跳过转译不必要的特性使得测试启动更快。

为了仅在测试时应用这些选项，可以把它们放到一个独立的 env.test 配置项中 (这会被 babel-jest 自动获取)。

.babelrc 文件示例：

    {
      "presets": [["env", { "modules": false }]],
      "env": {
        "test": {
          "presets": [["env", { "targets": { "node": "current" } }]]
        }
      }
    }

### 放置测试文件

默认情况下，Jest 将会递归的找到整个工程里所有 `.spec.js` 或 `.test.js` 扩展名的文件。如果这不符合你的需求，你也可以在 `package.json` 里的配置段落中改变它的 `testRegex`。

Jest 推荐你在被测试代码的所在目录下创建一个 `__tests__` 目录，但你也可以为你的测试文件随意设计自己习惯的文件结构。不过要当心 Jest 会为快照测试在临近测试文件的地方创建一个 `__snapshots__` 目录。

### 测试覆盖率

Jest 可以被用来生成多种格式的测试覆盖率报告。以下是一个简单的起步的例子：

扩展你的 `jest` 配置 (通常在 `package.json` 或 `jest.config.js` 中) 的 `collectCoverage` 选项，然后添加 `collectCoverageFrom` 数组来定义需要收集测试覆盖率信息的文件。

    {
      "jest": {
        // ...
        "collectCoverage": true,
        "collectCoverageFrom": ["**/*.{js,vue}", "!**/node_modules/**"]
      }
    }
    
这样就会开启默认格式的测试覆盖率报告。你可以通过 `coverageReporters` 选项来定制它们

    {
      "jest": {
        // ...
        "coverageReporters": ["html", "text-summary"]
      }
    }
# 用 `mocha-webpack`测试单文件组件
我们假定你在一开始已经安装并配置好了 webpack、vue-loader 和 Babel——例如通过 `vue-cli` 创建了 `webpack-simple` 模板脚手架。

首先要做的是安装测试依赖：
```
npm install --save-dev @vue/test-utils mocha mocha-webpack
```
接下来我们需要在 `package.json` 中定义一个测试脚本
```
// package.json
{
  "scripts": {
    "test": "mocha-webpack --webpack-config webpack.config.js --require test/setup.js test/**/*.spec.js"
  }
}
```
需要注意的事项：

1.`--webpack-config` 标识指定了该测试使用的 webpack 配置文件。在大多数情况下该配置会在其实际项目的配置文件基础上做一些小的调整。我们晚些时候会再聊到这一点。

2.`--require` 标识确保了文件 `test/setup.js` 会在任何测试之前运行，这样我们可以在该文件中设置测试所需的全局环境。

3.最后一个参数是该测试包所涵盖的所有测试文件的聚合
### 提取 webpack 配置
##### 暴露 NPM 依赖
在测试中我们很可能会导入一些 NPM 依赖——这里面的有些模块可能没有针对浏览器的场景编写，也不适合被 webpack 打包。另一个考虑是为了尽可能的将依赖外置以提升测试的启动速度。我们可以通过 `webpack-node-externals` 外置所有的 NPM 依赖：
```
// webpack.config.js
const nodeExternals = require('webpack-node-externals')

module.exports = {
  // ...
  externals: [nodeExternals()]
}
```
##### 源码表
```
module.exports = {
  // ...
  devtool: 'inline-cheap-module-source-map'
}
```
如果是在 IDE 中调试，我们推荐添加以下配置：
```
module.exports = {
  // ...
  output: {
    // ...
    // 在源码表中使用绝对路径 (对于在 IDE 中调试时很重要)
    devtoolModuleFilenameTemplate: '[absolute-resource-path]',
    devtoolFallbackModuleFilenameTemplate: '[absolute-resource-path]?[hash]'
  }
}
```
### 设置浏览器环境
Vue Test Utils 需要在浏览器环境中运行。我们可以在 Node 中使用 jsdom-global 进行模拟：
```
npm install --save-dev jsdom jsdom-global
```
然后在 `test/setup.js` 中写入：
```
require('jsdom-global')()
```
这行代码会在 Node 中添加一个浏览器环境，这样 Vue Test Utils 就可以正确运行了。
### 选用一个断言库
`Chai` 是一个流行的断言库，经常和 `Mocha` 配合使用。你可能也想把 `Sinon` 用于创建间谍和存根
这里我们将使用 `chai` 且令其全局可用，这样我们就不需要在每个测试文件里导入它了：
```
npm install --save-dev chai
```
然后在 `test/setup.js` 中编写：
```
require('jsdom-global')()
global.expect = require('chai')
```
### 为测试优化 Babel
注意我们使用了 `babel-loader` 来处理 JavaScript。如果你在你的应用中通过 `.babelrc` 文件使用了 Babel，那么你就已经算是把它配置好了。这里 babel-loader 将会自动使用相同的配置文件。

有一件事值得注意，如果你使用了 `Node 6+`，它已经支持了主要的 ES2015 特性，那么你可以配置一个独立的 Babel 环境选项，只转译该 Node 版本中不支持的特性 (比如 `stage-2` 或 `flow` 语法支持等)。
### 添加一个测试
在 `src` 目录中创建一个名为 `Counter.vue` 的文件：
```
<template>
  <div>
    {{ count }}
    <button @click="increment">自增</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        count: 0
      }
    },

    methods: {
      increment() {
        this.count++
      }
    }
  }
</script>
```
然后创建一个名为 `test/Counter.spec.js` 的测试文件并写入如下代码：
```
import { shallowMount } from '@vue/test-utils'
import Counter from '../src/Counter.vue'

describe('Counter.vue', () => {
  it('increments count when button is clicked', () => {
    const wrapper = shallowMount(Counter)
    wrapper.find('button').trigger('click')
    expect(wrapper.find('div').text()).to.be.equal('1')
  })
})
```
进行运行测试：
```
npm run test
```



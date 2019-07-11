---
title: requirejs压缩工具r.js的build.js配置详解
date: 2018-03-08 11:40:29
updated: 2019-02-03 14:21:03
comments: 1
categories: 前端工程化
tags: []
permalink: 22
---

至于requirejs大家都是很熟悉的了，它的打包工具r.js是非常好用，也非常强大的，但是建于它的配置参数比较多，这里列出一份所有参数的中文详解，方便理解和记忆。

<!--more-->
<table border="0" cellpadding="0" cellspacing="0">
<colgroup>
  <col style="width:400px">
  <col>
</colgroup>
<tbody>
  <tr>
    <td>
      <code>appDir: “some/path/”</code>
    </td>
    <td>
      <p>（选填）app的顶级目录。如果指定该参数，说明您的所有文件都在这个目录下面（包括baseUrl和dir都以这个为根目录）。如果不指定，则以baseUrl参数为准</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>baseUrl: ”./”</code>
    </td>
    <td>
      <p>模块所在默认相对目录。如果baseUrl指定不明确，所有被加载的模块都相对于生成目录。如果appDir被指定，baseUrl将相对于appDir目录。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>mainConfigFile: “../some/path/to/main.js”</code>
    </td>
    <td>
      <p>配置文件地址。默认所有的优化配置都在命令行或者配置文件里，而通过requirejs的data-main引入配置文件的方式是不起作用的。当然，如果你不想重复声明配置，可以直接通过这个参数指向data-main的文件。文件中的第一个requirejs({})，require({})，requirejs.config({})或者require.config({})方法调用会被用到。在2.1.10版本中，mainConfigFile可以是数组，并且后指定的值会覆盖先指定的值。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
paths: {
  "foo.bar": "../scripts/foo/bar",
  "baz": "../another/path/baz"
}
      </pre></code>
    </td>
    <td>
      <p>设置模块的路径。相对路径以baseUrl为当前目录。如果是”empty:”则指向一个空文件。使优化器在输出时不会包含该文件。对于指向CDN或者其它在浏览器中运行的 `http:URL` 的模块有用，在优化过程中它应该被跳过因为它没有依赖项。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>map: {}</code>
    </td>
    <td>
      <p>建立一个模块到其它模块的ID地图</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>packages: []</code>
    </td>
    <td>
      <p>配置CommonJS包，查看<a href="http://requirejs.org/docs/api.html#packages">http://requirejs.org/docs/api.html#packages</a></p>
    </td>
  </tr>
  <tr>
    <td>
      <code>dir: “../some/path”</code>
    </td>
    <td>
      <p>输出目录。如果不指定，默认会创建一个build目录，相对于生成文件所在的目录。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>keepBuildDir: false</code>
    </td>
    <td>
      <p>在2.0.2版本中，dir指向的目录在生成开始之前会被删除。如果你有一个大型的生成项目并且不会通过onBuildRead/onBuildWrite改变源文件，你可以使用这个参数来保持原目录，这样可以让重新生成时更快速，但是如果生成的代码被某种方式改变的话，可能会导致未知的错误。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>shim: {}</code>
    </td>
    <td>
      <p>如果shim配置在requirejs运行过程中被使用的话，需要在这里重复声明，这样才能将依赖模块正确引入。最好是使用mainConfigFile指定配置文件，这样就可以只在一个地方声明。当然，如果mainConfigFile没有声明，shim配置就必须声明。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>wrapShim: false</code>
    </td>
    <td>
      <p>在2.1.11版本中，shim的依赖可以包含在define()里。具体参见：<a href="http://requirejs.org/docs/api.html#config-shim">http://requirejs.org/docs/api.html#config-shim</a></p>
    </td>
  </tr>
  <tr>
    <td>
      <code>Locale: “en-us”</code>
    </td>
    <td>
      <p>使用i18n源文件指定语言</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>optimize: “uglify”</code>
    </td>
    <td>
      <p>默认会压缩所有js文件，closure|none</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>skipDirOptimize: false</code>
    </td>
    <td>
      <p>2.1.2中提到，如果使用dir作为输出目录，优化器会优化输出目录的所有JS（包括没有在modules配置中声明的）。当然，如果没有在modules里面声明的JS文件在生成过后不会被使用你可以跳过这些文件，以加快生成速度。将该参数设置为true来跳过这些不用被生成的JS文件。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>generateSourceMaps: false</code>
    </td>
    <td>
      <p>在2.1.2中是实验性质。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>normalizeDirDefines: “skip”</code>
    </td>
    <td>
      <p>2.1.11中：如果dir被声明且不为”none”，并且skipDirOptimize 为false，通常所有的JS文件都会被压缩，这个值自动设置为”all”。为了让JS文件能够在压缩过正确执行，优化器会加一层define()调用并且会插入一个依赖数组。当然，这样会有一点点慢如果有很多文件或者有大文件的时候。所以，设置该参数为”skip”这个过程就不会执行，如果optimize设置为”none”也一样。如果你想手动设置值的话：</p>
      <p>1）优化后：如果你打算压缩没在modules声明的JS文件，在优化器执行过后，你应该设置这个值为”all”</p>
      <p>2）优化中：但在动态加载过后，你想做一个会文件优化，但不打算在动态加载这些文件可以设置成”skip”</p>
      <p>最后：所有生成的文件（无论在不在modules里声明过）自动标准化</p>
    </td>
  </tr>
  <tr>
    <td>
<code><pre>
uglify: {
  toplevel: true,
  ascii_only: true,
  beautify: true,
  max_line_length: 1000,
  defines: {
    DEBUG: [“name”, “false”]
  },
  no_mangle: true
}
</pre></code>
    </td>
    <td>
      <p>如果用UglifyJS做优化，这些配置参数会被传递到UglifyJS，详情见：<a href="https://github.com/mishoo/UglifyJS">https://github.com/mishoo/UglifyJS</a></p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
Uglify2: {
  output: {
    beautify: true
  },
  compress: {
    sequences: false,
    global_defs: {
      DEBUG: false
    }
  },
  warnings: true,
  mangle: false
}
      </pre></code>
    </td>
    <td>
      <p>如果用UglifyJS2来优化，这些配置参数会被传入UglifyJS2</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
closure: {
  CompilerOption: {},
  CompilationLevel: “SIMPLE_OPTIMIZATIONS”,
  loggingLevel: “WANING”
}
      </pre></code>
    </td>
    <td>
      <p>如果用Closure Compiler优化，这个参数可以用来配置Closure Compiler，详细请看Closure Compiler的文档</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>optimizeCss: “standard.keepLines.keepWhitespace</code>
    </td>
    <td>
      <p>允许优化CSS，参数值：</p>
      <p>“standard”: @import引入并删除注释，删除空格和换行。删除换行在IE可能会出问题，取决于CSS的类型</p>
      <p>“standard.keepLines”: 和”standard”一样但是会保持换行</p>
      <p>“none”: 跳过CSS优化</p>
      <p>“standard.keepComments”: 保持注释，但是去掉换行(r.js 1.0.8+)</p>
      <p>“standard.keepComments.keepLines”: 保持注释和换行(r.js 1.0.8+)</p>
      <p>“standard.keepWhitespace”: 和”standard”一样但保持空格</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>cssImportIgnore: null</code>
    </td>
    <td>
      <p>如果optimizeCss可用，列出需要忽略@import的文件。值应该是以逗号分隔的CSS文件名（例：”a.css,b.css”，文件名必须和@import的相同</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>
      cssIn: “path/to/main.css”,<br>
      out: “path/to/css-optimized.css”
      </code>
    </td>
    <td>
      <p>cssIn是用在命令行的类型参数，它可以单独使用来优化单个CSS文件</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>cssPrefix: “”</code>
    </td>
    <td>
      <p>如果”out”和”cssIn”不是同一目录，并且在cssIn文件里面有url()相对目录的，用这个去设置URL前置。仅仅在优化后URL不正确的情况下使用。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>inlineText: true</code>
    </td>
    <td>
      <p>内联所有文本和依赖，避免多次异步请求这些依赖</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>userStrict: false</code>
    </td>
    <td>
      <p>允许“user strict”，用来包含RequireJS文件。默认为false是因为没有多少浏览器能够准确的处理ES5的strict mode错误，并且还有很多遗留代码在strict mode里面不能运行。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
pragmas: {
  fooExclude: true
}
      </pre></code>
    </td>
    <td>
      <p>指定生成编译指示。如果源文件包含类似如下注释：</p>
      <p>&gt;&gt;excludeStart(“fooExlude”, pragmas.fooExclude);</p>
      <p>&gt;&gt;excludeEnd(“fooExclude”);</p>
      <p>那么以//&gt;&gt;开头的注释就是编译指示。excludeStart/excludeEnd和includeStart/includeEnd起作用，在includeStart或excludeStart中的编译指示值将参与计算来判断Start和End之前的编译指示是include还是exclude。如果你可以选择用”has”或编译指示，建议用”has”代替。 编译指示比较难于阅读，但是它在对代码移除上比较灵活。基于”has”的代码必须遵守JavaScript规则。编译指示还可以在未压缩的代码中删除代码，而”has”只能通过UglifyJS或者Closure Compiler来做。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
pragmasOnSave: {
  excludeCoffeeScript: true
}
      </pre></code>
    </td>
    <td>
      <p>和”pragmas”一样，但只能在文件保存的优化阶段应用一次。”pragmas”可以同时在依赖映射和文件保存优化阶段应用。有些”pragmas”可能不会在依赖映射时被执行，例如在CoffeeScript的loader插件中，只想CoffeeScript做依赖映射，但是一旦这个文件被保存为一个javascript文件，CoffeeScript compiler就没用了。那样的话，pragmasOnSave就会用于在保存期排除编译代码。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
has: {
  “function-bind”: true,
  “string-trim”: false
}
      </pre></code>
    </td>
    <td>
      <p>使用”has”允许trimming代码树。基于js的特征检测：<a href="https://github.com/phiggins42/has.js">https://github.com/phiggins42/has.js</a>。代码树修饰仅仅在使用UglifyJS或Closure Compiler压缩时发生。更多请见：<a href="http://requirejs.org/docs/optimization.html#hasjs">http://requirejs.org/docs/optimization.html#hasjs</a></p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
hasOnSave: {
  “function-bind”: true,
  “string-trim”: false
}
      </pre></code>
    </td>
    <td>
      <p>和pragmasOnSave类似</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>namespace: “foo”</code>
    </td>
    <td>
      <p>允许requirejs名称空间，使require和define换作新的名字。更多见：<a href="http://requirejs.org/docs/faq-advanced.html#rename">http://requirejs.org/docs/faq-advanced.html#rename</a></p>
    </td>
  </tr>
  <tr>
    <td>
      <code>skipPragmas: false</code>
    </td>
    <td>
      <p>跳过执行pragmas</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>skipModuleInsertion: false</code>
    </td>
    <td>
      <p>如果是false，文件就不会用define()来定义模块而是用一个define()占位符插入其中。另外，require.pause/resume调用也会被插入。设置为”true”来避免。这个参数用在你不是用require()来创建项目或者写js文件，但是又想使用RquireJS的优化工具来合并模块是非常有用的。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>stubModules: [“text”, “bar”]</code>
    </td>
    <td>
      <p>将模块排除在优化文件之外。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>optimizeAllPluginResources: false</code>
    </td>
    <td>
      <p>如果不是一个文件的优化，描述输出目录的所有.js文件的插件依赖，如果这个插件支持优化成为一个单独的文件，就优化它。可能是一个比较慢的优化过程。仅仅在有些插件用了像XMLHttpRequest不支持跨域，并且生成的代码会被放在另一个域名。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>findNestedDependencies: false</code>
    </td>
    <td>
      <p align="left">寻找require()里面的require或define调用的依赖。默认为false是因为这些资源应该被认为是动态加载或者实时调用的。当然，有些优化场景也需要将它们合并在一起。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>removeCombined: false</code>
    </td>
    <td>
      <p>如果设置为true，在输出目录将会删除掉已经合并了的文件</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
modules: [
  {
    name: “foo/bar/bop”,
    create: true,
    override: {
      pragmas: {
        fooExclude: true
      }
    }
  },
  {
    name: “foo/bar/bop”,
    include: [“foo/bar/bee”]
  },
  {
    name: “foo/bar/bip”,
    exclude: [“foo/bar/bop”]
  },
  {
    name: “foo/bar/bin”,
    excludeShallow: [“foo/bar/bot”]
  },
  {
    name: “foo/barz”,
    insertRequire: [“foo/baz”]
  }
]
      </pre></code>
    </td>
    <td>
      <p>列出要优化的模块。如果有依赖i18n的，只有root层会被包含进来除非locale:块在上面被声明过。</p>
      <p>①仅定义模块会被转换成一个生成目标文件的名字，包含所有依赖项。i18n依赖同上，</p>
      <p>create：true可用来生成在源文件目录不存在的给定模块名。如果源文件目录已经存在一个相同名称的模块，create参数就没用了；</p>
      <p>override:可以重写全局的pragmas</p>
      <p>②这个模块声明编译foo/bar/bop的所有依赖和foo/bar/bee及其所有依赖</p>
      <p>③编译所有foo/bar/bip的依赖到一个文件，但是排除foo/bar/bop和它的所有依赖文件，如果想把另一个模块单独优化，这是一个很好用的方法</p>
      <p>④excludeShallow只排除掉这个模块，但是如果输出模块的依赖和它有相同就保留不排除。</p>
      <p>⑤这个模块声明表示用insertRequire（在2.0中新加入）</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>insertRequire: [“foo/bar/bop”]</code>
    </td>
    <td>
      <p>如果目标模块在顶层级只调用了define没有调用require()，并且输出文件在data-main中使用，如果顶层没有require，就不会有任何模块被加载。定义insertRequire在文件尾部来执行其它模块，更多参见：<a href="https://github.com/jrburke/almond">https://github.com/jrburke/almond</a></p>
    </td>
  </tr>
  <tr>
    <td>
      <code>
      name: “foo/bar/bop”,<br>
      include: [“foo/bar/bee”],<br>
      insertRequire: [“foo/bar/bop”],<br>
      out: “path/to/optimized-file.js”
      </code>
    </td>
    <td>
      <p>如果只优化一个模块（和它的依赖项），而且是生成一个单文件，你可以在行内定义模块的选项，以代替modules参数的定义方式，”exclude”, “excludeShallow”, “include”和”insertRquire”都可以以兄弟属性的方式定义</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>deps: [“foo/bar/bee”]</code>
    </td>
    <td>
      <p>“include”的替换方案。一般用requirejs.config()来定义并用mainConfigFile引入。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>out: function( text, sourceMapText ) { }</code>
    </td>
    <td>
      <p>在2.0，”out”可以是一个函数， 对单个JS文件优化可以调用requirejs.optimize()， 用out函数表示优化过后的内容不会被写到磁盘，而是传递给out函数。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>out: “stdout”</code>
    </td>
    <td>
      <p>在2.0.12+， 设置”out”为”stdout”， 优化输出会写到STDOUT，这对于r.js整合其它命令行工具很有用。为了避免额外的输出”logLevel: 4”应该被使用。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
wrap: {
  start: “(function() {“,
  end: “}())”
}
      </pre></code>
    </td>
    <td>
      <p>wrap任何东西在start和end之间，用于define/require不是全局的情况下，在end里可以暴露全局对象在文件中。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>wrap: true</code>
    </td>
    <td>
      <p>wrap的另一种方式，默认是(function() { + content + }())</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
wrap: {
  startFile: “parts/start.frag”,
  “endFile: “parts/end.frag”
}
      </pre></code>
    </td>
    <td>
      <p>用文件来wrap</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
wrap: {
  startFile: [
    “parts/startOne.frag”,
    “parts/startTwo.frag”
  ],
  endFile: [
    “parts/endOne.frag”,
    “parts/endTwo.frag”
  ]
}
      </pre></code>
    </td>
    <td>
      <p>多个文件的wrap</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>fileExclusionRegExp: /^\./</code>
    </td>
    <td>
      <p>跳过任何以.开头的目录和文件，比如.files, .htaccess等</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>preserveLicenseComments: true</code>
    </td>
    <td>
      <p>默认注释有授权在里面。当然，在大项目生成时，文件比较多，注释也比较多，这样可以把所有注释写在文件的顶部。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>logLevel: 0</code>
    </td>
    <td>
      <p>设置logLevel。</p>
      <p>TRACE: 0,</p>
      <p>INFO: 1</p>
      <p>WARN: 2</p>
      <p>ERROR: 3</p>
      <p>SILENT: 4</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>throwWhen: { optimize: true }</code>
    </td>
    <td>
      <p>在2.1.3，有些情况下当错误发生时不会抛出异常并停止优化，你可能想让优化器在某些错误发生时停止，就可以使用这个参数</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
onBuildRead: function( moduleName, path, contents) {
  return contents.replace(/foo/g, “bar”);
}
      </pre></code>
    </td>
    <td>
      <p>当每个文件被读取的时候调用这个方法来改变文件内容</p>
    </td>
  </tr>
  <tr>
    <td>
      <code><pre>
onBuildWrite: function( moduleName, path, contents) {
  return contents.replace(/bar/g, “foo”);
}
      </pre></code>
    </td>
    <td>
      <p>允许在写入目标文件前执行方法改变内容</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>onModuleBundleComplete: function( data) { }</code>
    </td>
    <td>
      <p>每个JS模块集完成后执行。 模块集是指一个modules数组项。</p>
    </td>
  </tr>

  <tr>
    <td>
      <code><pre>
rawText: {
  “some/id”: “define([‘another/id’], function () {} ); “
}
      </pre></code>
    </td>
    <td>
      <p>在2.1.3，种子raw text是模块ID的列表。这些文本内容用于代替模块的文件IO调用。用于模块ID是基于用户动态输入的情况，在网页生成工具中常用。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>cjsTranslate: true</code>
    </td>
    <td>
      <p>在2.0.2中。如果为true, 优化器会添加define(require, exports, module) {})；包裹每一个没有调用define()的文件。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>useSourceUrl: true</code>
    </td>
    <td>
      <p>在2.0.2，有点实验性质。每一个模块集最后都会添加一段//# sourceUrl的注释。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>waitSeconds: 7</code>
    </td>
    <td>
      <p>定义模块的加载时间。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>skipSemiColonInsertion: false</code>
    </td>
    <td>
      <p>在2.1.9，通常r.js插入一个分号在文件末尾，如果没有的话。</p>
    </td>
  </tr>
  <tr>
    <td>
      <code>keepAmdefine: false</code>
    </td>
    <td>
      <p>在2.1.10， 如果是true，就不会删除amdefine，详情见：<a href="https://github.com/jrburke/amdefine">https://github.com/jrburke/amdefine</a></p>
    </td>
  </tr>
  <tr>
    <td>
      <code>allowSourceOverwrites: false</code>
    </td>
    <td>
      <p>在2.1.11中， 作为修复BUG的一部分<a href="https://github.com/jrburke/r.js/issues/444">https://github.com/jrburke/r.js/issues/444</a>。设置为true就允许源代码进行重写覆盖。当然，为了安全起见，请正确配置，比如你可能想设置”keepBuildDir”为true。</p>
    </td>
  </tr>
</tbody>
</table>

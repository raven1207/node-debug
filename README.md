# 如何调试Node.js应用程序：技巧，窍门和工具

**软件开发非常复杂，并且有时您的Node.js应用程序将失败。如果\*幸运的话\*，您的代码将崩溃，并显示一条明显的错误消息。如果您不走运，您的应用程序将继续运行，无论是否产生预期的结果。如果您真的很倒霉，一切都会正常进行，直到第一个用户发现灾难性的磁盘擦除错误为止。**

## 什么是调试？

**调试**是修复软件缺陷的妖术。修复错误通常很容易-纠正字符或附加代码行即可解决问题。查找该错误是另一回事，开发人员可能会花费很多不愉快的时间来尝试查找问题的根源。幸运的是，Node.js具有一些出色的工具来帮助跟踪错误。

### 术语

调试有其自己的晦涩行话选择，包括以下内容：

|       术语 |                             说明                             |
| ---------: | :----------------------------------------------------------: |
|       断点 |           调试器停止程序的位置，以便可以检查其状态           |
|     调试器 |    提供调试工具的工具，例如逐行运行代码以检查内部变量状态    |
|       特征 | 就像声明中所说的：“这不是错误，而是功能”。所有开发人员都在其职业生涯中的某个时候说过 |
|       频率 |                     错误发生的频率或条件                     |
| 它不起作用 |                 最常提出但最不有用的错误报告                 |
|     记录点 |     给调试器的指令，以在执行过程中的某个时刻显示变量的值     |
|       测井 |                将运行时信息输出到控制台或文件                |
|   逻辑错误 |                  该程序有效，但未按预期运行                  |
|       优先 |               在计划的更新列表上分配错误的位置               |
|   比赛条件 |          难以跟踪的错误取决于不可控事件的顺序或时间          |
|       重构 |                 重写代码以提高可读性和维护性                 |
|       回归 |          可能由于其他更新而重新出现了先前修复的错误          |
|       有关 |                   与另一个相似或相关的错误                   |
|       复制 |                      导致错误所需的步骤                      |
|   RTFM错误 | 伪装成错误报告的用户权限不足，通常是对“阅读*翻转*手册” 的回应 |
|       踏进 |          在调试器中逐行运行代码时，进入被调用的函数          |
|     走出去 |        逐行运行时，请完成当前函数的执行并返回调用代码        |
|       跨过 |        逐行运行时，完成命令执行而无需进入其调用的函数        |
|   严重程度 | 错误对系统的影响。例如，除非发生频率非常低，否则通常认为数据丢失比UI问题更具问题。 |
|   堆栈跟踪 |             错误发生之前调用的所有函数的历史列表             |
|   语法错误 |                印刷错误，例如 `console.lug()`                |
|   用户错误 | 由用户而非应用程序引起的错误，但仍可能会根据该人员的资历进行更新 |
|         看 |                 在调试器执行期间要检查的变量                 |
|     观察点 |    与断点类似，不同之处在于当变量设置为特定值时程序会停止    |

## 如何避免虫子

在测试应用程序之前，通常可以防止错误……

### 使用好的代码编辑器

优秀的代码编辑器将提供许多功能，包括行编号，自动完成，颜色编码，括号匹配，格式化，自动缩进，变量重命名，代码段重用，对象检查，功能导航，参数提示，重构，无法访问的代码检测，建议，类型检查等。

免费的编辑器（例如[VS Code](https://code.visualstudio.com/)，[Atom](https://atom.io/)和[Brackets](http://brackets.io/)）以及大量的商业替代产品可让选择Node.js的开发者宠坏。

### 使用代码夹

在保存和测试代码之前，lint可能会报告代码错误，例如语法错误，缩进不佳，变量未声明以及括号不匹配。对于JavaScript和Node.js的热门选项包括[ESLint](https://eslint.org/)，[JSLint的](https://www.jslint.com/)，和[JSHint](https://jshint.com/)。

这些通常作为全局Node.js模块安装，因此您可以从命令行运行检查：

```bash
eslint myfile.js
```

然而，大多数棉短绒有代码编辑器插件，如[ESLint的VS代码](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)和[棉短绒，eslint Atom的](https://atom.io/packages/linter-eslint)这些检查会在您输入的代码：

![ESLint for VS代码](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581435166eslint.png)

### 使用源代码管理

诸如[Git之](https://git-scm.com/)类的源代码控制系统可以帮助保护您的代码并管理修订。发现错误的位置和时间以及由谁来负责更容易发现！[GitHub](https://github.com/)和[Bitbucket](https://bitbucket.org/)等在线存储库提供免费空间和管理工具。

### 采用问题跟踪系统

*如果没人知道，是否存在错误？*问题跟踪系统用于报告错误，查找重复项，文档复制步骤，确定严重性，计算优先级，分配开发人员，记录讨论并跟踪所有修复的进度。

在线资源库通常提供基本的问题跟踪，但是[专用的解决方案](https://en.wikipedia.org/wiki/Comparison_of_issue-tracking_systems)可能适用于较大的团队和项目。

### 使用测试驱动的开发

[测试驱动开发](https://www.sitepoint.com/learning-javascript-test-driven-development-by-example/)（TDD）是一个发展的过程，鼓励开发人员测试该功能之前它写操作的写代码-例如，*当函数Y传递输入Z X返回*。



在开发代码时可以运行测试，以证明功能可以正常工作，并在进行进一步更改时发现任何问题。也就是说，您的测试也可能存在错误……

### 走开

试图整夜熬夜，徒劳地尝试查找令人讨厌的bug的来源。别。走开，做其他事情。您的大脑将下意识地解决问题，并在凌晨4点通过解决方案将您唤醒。即使这没有发生，新鲜的眼睛也会发现明显的缺失分号。

## Node.js调试：环境变量

主机操作系统中设置的[环境变量](https://nodejs.org/api/cli.html#cli_environment_variables)可用于控制Node.js应用程序设置。最常见的是`NODE_ENV`，通常`development`在调试时设置为。

可以在Linux / macOS上设置环境变量：

```bash
NODE_ENV=development
```

Windows `cmd`：

```bash
set NODE_ENV=development
```

或Windows Powershell：

```bash
$env:NODE_ENV="development"
```

在内部，应用程序将启用进一步的调试功能和消息。例如：

```javascript
// is NODE_ENV set to "development"?
const DEVMODE = (process.env.NODE_ENV === 'development');

if (DEVMODE) {
  console.log('application started in development mode on port ${PORT}');
}
NODE_DEBUG`启用使用Node.js调试消息（请参见下文），但也请查阅主要模块和框架的文档以发现更多选项。`util.debuglog
```

请注意，环境变量也可以保存到文件中。例如：`.env`

```env
NODE_ENV=development
NODE_LOG=./log/debug.log
SERVER_PORT=3000
DB_HOST=localhost
DB_NAME=mydatabase
```

然后使用[`dotenv`模块](https://www.npmjs.com/package/dotenv)加载：

```javascript
require('dotenv').config();
```

## Node.js调试：命令行选项

启动应用程序时，可以将各种[命令行选项](https://nodejs.org/api/cli.html)传递给`node`运行时。最有用的方法之一是，它输出用于过程警告（包括弃用）的堆栈跟踪。`--trace-warnings`

可以设置任何数量的选项，包括：

- `--enable-source-maps`：启用源地图（实验性）
- `--throw-deprecation`：使用不推荐使用的功能时引发错误
- `--inspect`：激活V8检查器（见下文）

通过示例，让我们尝试记录[加密模块的`DEFAULT_ENCODING`属性](https://nodejs.org/api/crypto.html#crypto_crypto_default_encoding)，该[属性](https://nodejs.org/api/crypto.html#crypto_crypto_default_encoding)在节点v10中已弃用：



```javascript
const crypto = require('crypto');

function bar() {
  console.log(crypto.DEFAULT_ENCODING);
}

function foo(){
  bar();
}

foo();
```

现在，使用以下命令运行它：

```bash
node index.js
```

然后，我们将看到以下内容：

```text
buffer
(node:7405) [DEP0091] DeprecationWarning: crypto.DEFAULT_ENCODING is deprecated.
```

但是，我们也可以这样做：

```bash
node --trace-warnings index.js
```

产生以下内容：

```text
buffer
(node:7502) [DEP0091] DeprecationWarning: crypto.DEFAULT_ENCODING is deprecated.
    at bar (/home/Desktop/index.js:4:22)
    at foo (/home/Desktop/index.js:8:3)
    at Object.<anonymous> (/home/Desktop/index.js:11:1)
    at Module._compile (internal/modules/cjs/loader.js:1151:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1171:10)
    at Module.load (internal/modules/cjs/loader.js:1000:32)
    at Function.Module._load (internal/modules/cjs/loader.js:899:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:71:12)
    at internal/main/run_main_module.js:17:47
```

这告诉我们，弃用警告来自第4行（该语句）中的代码，该代码在函数运行时执行。该函数由第8行的函数调用，并且该函数在脚本的第11行被调用。`console.log``bar``bar``foo``foo`

注意，相同的选项也可以传递给[nodemon](https://nodemon.io/)。

## 控制台调试

调试应用程序最简单的方法之一是在执行期间将值输出到控制台：

```javascript
console.log( myVariable );
```

很少有开发人员会尝试使用此不起眼的调试命令，但是他们却错过了[更多的可能性](https://nodejs.org/api/console.html)，包括：

|                                               `console` 方法 |                             描述                             |
| -----------------------------------------------------------: | :----------------------------------------------------------: |
| [`.log(msg)`](https://nodejs.org/api/console.html#console_console_log_data_args) |                       向控制台输出消息                       |
| [`.dir(obj,opt)`](https://nodejs.org/api/console.html#console_console_dir_obj_options) | 用于漂亮地打印对象和属性[`util.inspect`](https://nodejs.org/api/util.html#util_util_inspect_object_options) |
| [`.table(obj)`](https://nodejs.org/api/console.html#console_console_table_tabulardata_properties) |                    以表格格式输出对象数组                    |
| [`.error(msg)`](https://nodejs.org/api/console.html#console_console_error_data_args) |                         输出错误信息                         |
| [`.count(label)`](https://nodejs.org/api/console.html#console_console_count_label) |              一个命名计数器，报告执行该行的次数              |
| [`.countReset[label\]`](https://nodejs.org/api/console.html#console_console_countreset_label) |                        重置命名计数器                        |
| [`.group(label)`](https://nodejs.org/api/console.html#console_console_group_label) |                       缩进一组日志消息                       |
| [`.groupEnd(label)`](https://nodejs.org/api/console.html#console_console_groupend) |                          结束缩进组                          |
| [`.time(label)`](https://nodejs.org/api/console.html#console_console_time_label) |                 启动计时器以计算操作持续时间                 |
| [`.timeLog([label\]`](https://nodejs.org/api/console.html#console_console_timelog_label_data) |                报告自计时器启动以来经过的时间                |
| [`.timeEnd(label)`](https://nodejs.org/api/console.html#console_console_timeend_label) |                  停止计时器并报告总持续时间                  |
| [`.trace()`](https://nodejs.org/api/console.html#console_console_trace_message_args) |              输出堆栈跟踪（所有调用函数的列表）              |
| [`.clear()`](https://nodejs.org/api/console.html#console_console_clear) |                          清除控制台                          |

`console.log()`接受逗号分隔值的列表。例如：

```javascript
let x = 123;
console.log('x:', x);
// x: 123
```

但是，[ES6解构](https://www.sitepoint.com/es6-destructuring-assignment/)可以以更少的打字工作提供类似的输出：

```javascript
console.log({x});
// { x: 123 }
```

可以使用以下命令将较大的对象输出为压缩字符串：

```javascript
console.log( JSON.stringify(obj) );
```

[`util.inspect`](https://nodejs.org/api/util.html#util_util_inspect_object_options)将格式化对象以便于阅读，但会为您带来辛苦的工作。`console.dir()`

## Node.js `util.debuglog`

Node.js `util`模块提供了一种内置[`debuglog`](https://nodejs.org/api/util.html#util_util_debuglog_section)方法，该方法有条件地将消息写入`STDERR`：



```javascript
const util = require('util');
const debuglog = util.debuglog('myapp');

debuglog('myapp debug message [%d]', 123);
```

当`NODE_DEBUG`环境变量设置为`myapp`（或通配符，例如`*`或）时，消息将显示在控制台中：`my*`

```bash
NODE_DEBUG=myapp node index.js
MYAPP 9876: myapp debug message [123]
```

这`9876`是Node.js进程ID。

默认情况下为静音。如果在不设置变量的情况下运行上述脚本，则不会向控制台输出任何内容。这使您可以在代码中保留有用的调试日志，而不会使控制台混乱，无法正常使用。`util.debuglog``NODE_DEBUG`

## 使用日志模块进行调试

如果您需要更高级的消息传递级别，详细程度，排序，文件输出，概要分析等选项，则可以使用第三方日志记录模块。热门选项包括：

- [cabin](https://www.npmjs.com/package/cabin)
- [loglevel](https://www.npmjs.com/package/loglevel)
- [morgan](https://www.npmjs.com/package/morgan) (Express.js middleware)
- [pino](https://www.npmjs.com/package/pino)
- [signale](https://www.npmjs.com/package/signale)
- [storyboard](https://www.npmjs.com/package/storyboard)
- [tracer](https://www.npmjs.com/package/tracer)
- [winston](https://www.npmjs.com/package/winston)


## Node.js V8检查器

在以下各节中，将使用[其他教程中](https://www.sitepoint.com/a-beginner-splurge-in-node-js/)开发的[pagehit项目](https://github.com/sitepoint-editors/pagehit-ram)来说明调试概念。您可以通过以下方式下载：

```bash
git clone https://github.com/sitepoint-editors/pagehit-ram
```

或者，您可以使用任何自己的代码。

Node.js是V8 JavaScript引擎的包装，它包括自己的[检查器和调试客户端](https://nodejs.org/api/debugger.html)。首先，使用`inspect`参数（不要与混淆`--inspect`）来启动应用程序：

```bash
node inspect ./index.js
```

调试器将在第一行暂停并显示提示：`debug>`

```bash
< Debugger listening on ws://127.0.0.1:9229/6f38abc1-8568-4035-a5d2-dee6cbbf7e44
< For help, see: https://nodejs.org/en/docs/inspector
< Debugger attached.
Break on start in index.js:7
  5 const
  6   // default HTTP port
> 7   port = 3000,
  8
  9   // Node.js modules
debug>
```

您可以输入以下内容逐步浏览应用程序：

- `cont`或`c`：继续执行
- `next`或`n`：运行下一个命令
- `step`或`s`：进入被调用的函数
- `out`或`o`：退出功能并返回到调用命令
- `pause`：暂停运行代码

其他选项包括：

- 观察变量值 [`watch('myvar')`](https://nodejs.org/api/debugger.html#debugger_watchers)
- 使用[/ ](https://nodejs.org/api/debugger.html#debugger_breakpoints)[命令](https://nodejs.org/api/debugger.html#debugger_breakpoints)设置断点（通常更容易在代码中插入一条语句）[`setBreakpoint()``sb()`](https://nodejs.org/api/debugger.html#debugger_breakpoints)`debugger;`
- `restart` 一个剧本
- `.exit`调试器（`.`必须是缩写）

没得选时，才使用内置的debug客户端，虽然感到特别蛋疼。并且在您没有使用Windows情况下（这经常有问题）。

## 使用Chrome进行Node.js调试

Node.js检查器（没有调试器客户端）以以下`--inspect`标志启动：

```bash
node --inspect ./index.js
```

*注意：如有必要，`nodemon`可以代替使用`node`。*



这将启动侦听调试器，任何本地调试客户端都可以将其附加到：`127.0.0.1:9229`

```bash
Debugger listening on ws://127.0.0.1:9229/20ac75ae-90c5-4db6-af6b-d9d74592572f
```

如果您正在其他设备或Docker容器上运行Node.js应用程序，请确保`9229`可访问端口并使用以下方法授予远程访问权限：

```bash
node --inspect=0.0.0.0:9229 ./index.js
```

或者，您可以使用在第一条语句上设置断点，以便应用程序立即暂停。`--inspect-brk`

打开Chrome，然后在地址栏中输入。`chrome://inspect`

![镀铬检查](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523889chrome-inspect.png)

*注意：如果Node.js应用程序未显示为“ **远程目标”**，请确保选中“ **发现网络目标”**，然后单击“ **配置”**以添加运行该应用程序的设备的IP地址和端口。*

单击目标的**检查**链接以启动DevTools。具有浏览器调试经验的任何人都将立即熟悉它。

![Chrome DevTools](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523888chrome-devtools.png)

通过**+将文件夹添加到工作区**链接，您可以选择Node.js文件在系统上的位置，因此可以更轻松地加载其他模块并进行更改。

单击任何行号都会设置一个断点，该断点由绿色标记表示，在到达该代码时将停止执行：

![Chrome DevTools断点](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523886chrome-breakpoint.png)

通过单击**+**图标并输入其名称，可以将变量添加到右侧的“ **监视”**窗格中。只要暂停执行，就会显示它们的值。

“ **调用堆栈”**窗格显示达到该点需要调用的函数。



“ **范围”**窗格显示所有可用的局部变量和全局变量的状态。

“ **断点”**窗格显示所有断点的列表，并允许启用或禁用它们。

**调试器已暂停**消息上方的图标可用于恢复执行，进入，进入，退出，逐步执行，停用所有断点以及在异常上暂停。

## 使用VS代码进行Node.js调试

在本地系统上运行Node.js应用程序时，无需进行任何配置即可启动VS Code Node.js调试。打开起始文件（通常为），激活“ **运行和调试”**窗格，然后单击“ **运行并调试Node.js（F5）”**按钮。`index.js`

![VS代码调试器](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523892vscode-debug.png)

调试屏幕类似于Chrome DevTools，其中包含**Variables**，**Watch**，**Call stack**，**Loaded scripts**和**Breakpoints**列表。

![VS代码断点](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523890vscode-breakpoint.png)

可以通过单击行号旁边的装订线来设置断点。您也可以单击鼠标右键。

![VS Code断点选项](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523897vscode-points.png)

右键单击，可以设置以下内容：

1. 一个标准的断点。

2. 当满足条件时停止的条件断点，例如。`count > 3`

3. 一个日志点，实际上没有代码！可以使用大括号表示的表达式输入任何字符串，例如，以显示变量的值。`console.log()``{count}``count`

   

   ![VS代码日志点](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523896vscode-logpoint.png)

*注意：不要忘记点击ReturnVS Code来创建条件断点或日志点。*

顶部的调试图标栏可用于恢复执行，单步执行，进入，退出，重新启动或停止应用程序和调试。菜单中的“ **调试”**项也提供了相同的选项。

有关更多信息，请参考[在Visual Studio Code中调试](https://code.visualstudio.com/docs/introvideos/debugging)。

### 高级调试配置

当您调试远程服务或需要使用其他启动选项时，需要进一步配置。VS Code将启动配置存储在项目内文件夹内生成的文件中。要生成或编辑文件，请单击“ **运行和调试”**窗格右上方的齿轮图标。`launch.json``.vscode`

![VS Code启动配置](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523893vscode-launch.png)

可以将任何数量的配置设置添加到`configurations`阵列。单击**添加配置**按钮以选择一个选项。VS Code可以：

1. 使用Node.js本身**启动**进程，或者
2. **附加**到可能运行在远程计算机或Docker容器上的Node.js检查器进程

在上面的示例中，已经定义了一个Nodemon启动配置。保存，从“ **运行和调试”**窗格顶部的下拉列表中选择，然后单击绿色的开始图标。`launch.json``nodemon`

![VS Code启动](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523895vscode-launchconfig.png)

有关更多信息，请参见[VS Code Launch配置](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations)。

## 其他Node.js调试工具

在[Node.js的调试指南](https://nodejs.org/en/docs/guides/debugging-getting-started/)提供了其他IDE和编辑，包括Visual Studio中，JetBrains公司，WebStorm，Gitpod和Eclipse建议。Atom还具有[节点调试](https://atom.io/packages/node-debug)扩展名。

[ndb](https://github.com/GoogleChromeLabs/ndb)通过强大的功能（例如附加到子进程和脚本[黑匣子）](https://github.com/GoogleChromeLabs/ndb)提供了*改进的调试体验*，因此仅显示特定文件夹中的代码。

使用该选项运行时，[用于Node.js](https://github.com/ibm/report-toolkit)的[IBM report-toolkit](https://github.com/ibm/report-toolkit)通过分析数据输出[来](https://github.com/ibm/report-toolkit)工作。`node``--experimental-report`

最后，诸如[LogRocket](https://logrocket.com/)和[Sentry.io之](https://sentry.io/)类的商业服务与客户端和服务器中的实时Web应用程序集成在一起，以记录用户遇到的错误。

## 进行调试！

Node.js具有一系列出色的调试工具和代码分析器，可提高应用程序的速度和可靠性。他们是否能吸引您远离是另一回事！`console.log()`

[![img](https://secure.gravatar.com/avatar/439aeaff7de2bae365adc3eb4947b44d?s=96&d=mm&r=g)](https://www.sitepoint.com/author/craig-buckler/)

认识作者

[克雷格·巴克勒 ](https://www.sitepoint.com/author/craig-buckler/)

Craig是一位自由的英国Web顾问，他于1995年为IE2.0建立了他的第一页。自那时以来，他一直在倡导标准，可访问性和最佳实践HTML5技术。他为公司和组织（包括英国议会，欧洲议会，能源与气候变化部，微软等）创建了企业规范，网站和在线应用程序。他为SitePoint写了1000多篇文章，您可以找到他[@craigbuckler](http://twitter.com/craigbuckler)。


# How to Debug a Node.js Application: Tips, Tricks and Tools

**Software development is complex and, at some point, your Node.js application will fail. If you’re \*lucky\*, your code will crash with an obvious error message. If you’re unlucky, your application will carry on regardless but not generate the results you expect. If you’re really unlucky, everything will work fine until the first user discovers a catastrophic disk-wiping bug.**

## What is Debugging?

**Debugging** is the black art of fixing software defects. Fixing a bug is often easy — a corrected character or additional line of code solves the problem. Finding that bug is another matter, and developers can spend many unhappy hours trying to locate the source of an issue. Fortunately, Node.js has some great tools to help trace errors.

### Terminology

Debugging has its own selection of obscure jargon, including the following:

|            Term |                         Explanation                          |
| --------------: | :----------------------------------------------------------: |
|      breakpoint | the point at which a debugger stops a program so its state can be inspected |
|        debugger | a tool which offers debugging facilities such as running code line by line to inspect internal variable states |
|         feature | as in the claim: “it’s not a bug, it’s a feature”. All developers say it at some point during their career |
|       frequency |     how often or under what conditions a bug will occur      |
| it doesn’t work |       the most-often made but least useful bug report        |
|       log point | an instruction to a debugger to show the value of a variable at a point during execution |
|         logging |    output of runtime information to the console or a file    |
|     logic error |        the program works but doesn’t act as intended         |
|        priority |    where a bug is allocated on a list of planned updates     |
|  race condition | hard-to-trace bugs dependent the sequence or timing of uncontrollable events |
|     refactoring |      rewriting code to help readability and maintenance      |
|      regression | re-emergence of a previously fixed bug perhaps owing to other updates |
|         related |         a bug which is similar or related to another         |
|       reproduce |            the steps required to cause the error             |
|      RTFM error | user incompetence disguised as a bug report, typically followed by a response to “Read The *Flipping* Manual” |
|       step into | when running code line by line in a debugger, step into the function being called |
|        step out | when running line by line, complete execution of the current function and return to the calling code |
|       step over | when running line by line, complete execution of a command without stepping into a function it calls |
|        severity | the impact of a bug on system. For example, data loss would normally be considered more problematic than a UI issue unless the frequency of occurrence is very low |
|     stack trace | the historical list of all functions called before the error occurred |
|    syntax error |        typographical errors, such as `console.lug()`         |
|      user error | an error caused by a user rather than the application, but may still incur an update depending on that person’s seniority |
|           watch |       a variable to examine during debugger execution        |
|      watchpoint | similar to a breakpoint, except the program is stopped when a variable is set to a specific value |

## How to Avoid Bugs

Bugs can often be prevented before you test your application …

### Use a Good Code Editor

A good code editor will offer numerous features including line numbering, auto-completion, color-coding, bracket matching, formatting, auto-indentation, variable renaming, snippet reuse, object inspection, function navigation, parameter prompts, refactoring, unreachable code detection, suggestions, type checking, and more.

Node.js devs are spoiled for choice with free editors such as [VS Code](https://code.visualstudio.com/), [Atom](https://atom.io/), and [Brackets](http://brackets.io/), as well as plenty of commercial alternatives.

### Use a Code Linter

A linter can report code faults such as syntax errors, poor indentation, undeclared variables, and mismatching brackets before you save and test your code. The popular options for JavaScript and Node.js include [ESLint](https://eslint.org/), [JSLint](https://www.jslint.com/), and [JSHint](https://jshint.com/).

These are often installed as global Node.js modules so you can run checks from the command line:

```bash
eslint myfile.js
```

However, most linters have code editor plugins, such as [ESLint for VS Code](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) and [linter-eslint for Atom](https://atom.io/packages/linter-eslint) which check your code as you type:

![ESLint for VS Code](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581435166eslint.png)

### Use Source Control

A source control system such as [Git](https://git-scm.com/) can help safe-guard your code and manage revisions. It becomes easier to discover where and when a bug was introduced and who should receive the blame! Online repositories such as [GitHub](https://github.com/) and [Bitbucket](https://bitbucket.org/) offer free space and management tools.

### Adopt an Issue-tracking System

*Does a bug exist if no one knows about it?* An issue-tracking system is used to report bugs, find duplicates, document reproduction steps, determine severity, calculate priorities, assign developers, record discussions, and track progress of any fixes.

Online source repositories often offer basic issue tracking, but [dedicated solutions](https://en.wikipedia.org/wiki/Comparison_of_issue-tracking_systems) may be appropriate for larger teams and projects.

### Use Test-driven Development

[Test-driven Development](https://www.sitepoint.com/learning-javascript-test-driven-development-by-example/) (TDD) is a development process which encourages developers to write code which tests the operation of a function before it’s written — for example, *is X returned when function Y is passed input Z*.



Tests can be run as the code is developed to prove a function works and spot any issues as further changes are made. That said, your tests could have bugs too …

### Step Away

It’s tempting to stay up all night in a futile attempt to locate the source of a nasty bug. Don’t. Step away and do something else. Your brain will subconsciously work on the problem and wake you at 4am with a solution. Even if that doesn’t happen, fresh eyes will spot that obvious missing semicolon.

## Node.js Debugging: Environment Variables

[Environment variables](https://nodejs.org/api/cli.html#cli_environment_variables) that are set within the host operating system can be used to control Node.js application settings. The most common is `NODE_ENV`, which is typically set to `development` when debugging.

Environment variables can be set on Linux/macOS:

```bash
NODE_ENV=development
```

Windows `cmd`:

```bash
set NODE_ENV=development
```

Or Windows Powershell:

```bash
$env:NODE_ENV="development"
```

Internally, an application will enable further debugging features and messages. For example:

```javascript
// is NODE_ENV set to "development"?
const DEVMODE = (process.env.NODE_ENV === 'development');

if (DEVMODE) {
  console.log('application started in development mode on port ${PORT}');
}
```

`NODE_DEBUG` enables debugging messages using the Node.js `util.debuglog` (see below), but also consult the documentation of your primary modules and frameworks to discover further options.

Note that environment variables can also be saved to a `.env` file. For example:

```env
NODE_ENV=development
NODE_LOG=./log/debug.log
SERVER_PORT=3000
DB_HOST=localhost
DB_NAME=mydatabase
```

Then loaded using the [`dotenv` module](https://www.npmjs.com/package/dotenv):

```javascript
require('dotenv').config();
```

## Node.js Debugging: Command Line Options

Various [command-line options](https://nodejs.org/api/cli.html) can be passed to the `node` runtime when launching an application. One of the most useful is `--trace-warnings`, which outputs stack traces for process warnings (including deprecations).

Any number of options can be set, including:

- `--enable-source-maps`: enable source maps (experimental)
- `--throw-deprecation`: throw errors when deprecated features are used
- `--inspect`: activate the V8 inspector (see below)

By way of an example, let’s try to log [the crypto module’s `DEFAULT_ENCODING` property](https://nodejs.org/api/crypto.html#crypto_crypto_default_encoding), which was deprecated in Node v10:



```javascript
const crypto = require('crypto');

function bar() {
  console.log(crypto.DEFAULT_ENCODING);
}

function foo(){
  bar();
}

foo();
```

Now run this with the following:

```bash
node index.js
```

We’ll then see this:

```text
buffer
(node:7405) [DEP0091] DeprecationWarning: crypto.DEFAULT_ENCODING is deprecated.
```

However, we can also do this:

```bash
node --trace-warnings index.js
```

That produces the following:

```text
buffer
(node:7502) [DEP0091] DeprecationWarning: crypto.DEFAULT_ENCODING is deprecated.
    at bar (/home/Desktop/index.js:4:22)
    at foo (/home/Desktop/index.js:8:3)
    at Object.<anonymous> (/home/Desktop/index.js:11:1)
    at Module._compile (internal/modules/cjs/loader.js:1151:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1171:10)
    at Module.load (internal/modules/cjs/loader.js:1000:32)
    at Function.Module._load (internal/modules/cjs/loader.js:899:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:71:12)
    at internal/main/run_main_module.js:17:47
```

This tells us that the deprecation warning comes from the code in line 4 (the `console.log` statement), which was executed when the `bar` function ran. The `bar` function was called by the `foo` function on line 8 and the `foo` function was called on line 11 of our script.

Note that the same options can also be passed to [nodemon](https://nodemon.io/).

## Console Debugging

One of the easiest ways to debug an application is to output values to the console during execution:

```javascript
console.log( myVariable );
```

Few developers delve beyond this humble debugging command, but they’re missing out on [many more possibilities](https://nodejs.org/api/console.html), including these:

|                                             `console` method |                         description                          |
| -----------------------------------------------------------: | :----------------------------------------------------------: |
| [`.log(msg)`](https://nodejs.org/api/console.html#console_console_log_data_args) |               output a message to the console                |
| [`.dir(obj,opt)`](https://nodejs.org/api/console.html#console_console_dir_obj_options) | uses [`util.inspect`](https://nodejs.org/api/util.html#util_util_inspect_object_options) to pretty-print objects and properties |
| [`.table(obj)`](https://nodejs.org/api/console.html#console_console_table_tabulardata_properties) |         outputs arrays of objects in tabular format          |
| [`.error(msg)`](https://nodejs.org/api/console.html#console_console_error_data_args) |                   output an error message                    |
| [`.count(label)`](https://nodejs.org/api/console.html#console_console_count_label) | a named counter reporting the number of times the line has been executed |
| [`.countReset[label\]`](https://nodejs.org/api/console.html#console_console_countreset_label) |                    resets a named counter                    |
| [`.group(label)`](https://nodejs.org/api/console.html#console_console_group_label) |               indents a group of log messages                |
| [`.groupEnd(label)`](https://nodejs.org/api/console.html#console_console_groupend) |                   ends the indented group                    |
| [`.time(label)`](https://nodejs.org/api/console.html#console_console_time_label) |   starts a timer to calculate the duration of an operation   |
| [`.timeLog([label\]`](https://nodejs.org/api/console.html#console_console_timelog_label_data) |       reports the elapsed time since the timer started       |
| [`.timeEnd(label)`](https://nodejs.org/api/console.html#console_console_timeend_label) |        stops the timer and reports the total duration        |
| [`.trace()`](https://nodejs.org/api/console.html#console_console_trace_message_args) |   outputs a stack trace (a list of all calling functions)    |
| [`.clear()`](https://nodejs.org/api/console.html#console_console_clear) |                      clear the console                       |

`console.log()` accepts a list of comma-separated values. For example:

```javascript
let x = 123;
console.log('x:', x);
// x: 123
```

However, [ES6 destructuring](https://www.sitepoint.com/es6-destructuring-assignment/) can offer similar output with less typing effort:

```javascript
console.log({x});
// { x: 123 }
```

Larger objects can be output as a condensed string using this:

```javascript
console.log( JSON.stringify(obj) );
```

[`util.inspect`](https://nodejs.org/api/util.html#util_util_inspect_object_options) will format objects for easier reading, but `console.dir()` does the hard work for you.

## Node.js `util.debuglog`

The Node.js `util` module offers a built-in [`debuglog`](https://nodejs.org/api/util.html#util_util_debuglog_section) method which conditionally writes messages to `STDERR`:



```javascript
const util = require('util');
const debuglog = util.debuglog('myapp');

debuglog('myapp debug message [%d]', 123);
```

When the `NODE_DEBUG` environment variable is set to `myapp` (or a wildcard such as `*` or `my*`), messages are displayed in the console:

```bash
NODE_DEBUG=myapp node index.js
MYAPP 9876: myapp debug message [123]
```

Here, `9876` is the Node.js process ID.

By default, `util.debuglog` is silent. If you were to run the above script without setting a `NODE_DEBUG` variable, nothing would be output to the console. This allows you to leave helpful debug logging in your code without cluttering the console for regular use.

## Debugging with Log Modules

Third-party logging modules are available should you require more sophisticated options for messaging levels, verbosity, sorting, file output, profiling, and more. Popular options include:

- [cabin](https://www.npmjs.com/package/cabin)
- [loglevel](https://www.npmjs.com/package/loglevel)
- [morgan](https://www.npmjs.com/package/morgan) (Express.js middleware)
- [pino](https://www.npmjs.com/package/pino)
- [signale](https://www.npmjs.com/package/signale)
- [storyboard](https://www.npmjs.com/package/storyboard)
- [tracer](https://www.npmjs.com/package/tracer)
- [winston](https://www.npmjs.com/package/winston)

## Node.js V8 Inspector

In the following sections, the [pagehit project](https://github.com/sitepoint-editors/pagehit-ram) developed in [other tutorials](https://www.sitepoint.com/a-beginner-splurge-in-node-js/) is used to illustrate debugging concepts. You can download it with:

```bash
git clone https://github.com/sitepoint-editors/pagehit-ram
```

Or you can use any of your own code.

Node.js is a wrapper around the V8 JavaScript engine which includes its own [inspector and debugging client](https://nodejs.org/api/debugger.html). To start, use the `inspect` argument (not to be confused with `--inspect`) to start an application:

```bash
node inspect ./index.js
```

The debugger will pause at the first line and display a `debug>` prompt:

```bash
< Debugger listening on ws://127.0.0.1:9229/6f38abc1-8568-4035-a5d2-dee6cbbf7e44
< For help, see: https://nodejs.org/en/docs/inspector
< Debugger attached.
Break on start in index.js:7
  5 const
  6   // default HTTP port
> 7   port = 3000,
  8
  9   // Node.js modules
debug>
```

You can step through the application by entering:

- `cont` or `c`: continue execution
- `next` or `n`: run the next command
- `step` or `s`: step into a function being called
- `out` or `o`: step out of a function and return to the calling command
- `pause`: pause running code

Other options include:

- watching variable values with [`watch('myvar')`](https://nodejs.org/api/debugger.html#debugger_watchers)
- setting breakpoints with the [`setBreakpoint()`/`sb()` command](https://nodejs.org/api/debugger.html#debugger_breakpoints) (it is usually easier to insert a `debugger;` statement in your code)
- `restart` a script
- `.exit` the debugger (the initial `.` is required)

If this sounds horribly clunky, *it is*. Only use the built-in debugging client when there’s absolutely no other option, you’re feeling particularly masochistic, and you’re not using Windows (it’s often problematic).

## Node.js Debugging with Chrome

The Node.js inspector (without the debugger client) is started with the `--inspect` flag:

```bash
node --inspect ./index.js
```

*Note: `nodemon` can be used instead of `node` if necessary.*



This starts the debugger listening on `127.0.0.1:9229`, which any local debugging client can attach to:

```bash
Debugger listening on ws://127.0.0.1:9229/20ac75ae-90c5-4db6-af6b-d9d74592572f
```

If you’re running the Node.js application on another device or Docker container, ensure port `9229` is accessible and grant remote access using this:

```bash
node --inspect=0.0.0.0:9229 ./index.js
```

Alternatively, you can use `--inspect-brk` to set a breakpoint on the first statement so the application is paused immediately.

Open Chrome and enter `chrome://inspect` in the address bar.

![Chrome inspect](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523889chrome-inspect.png)

*Note: if the Node.js application does’t appear as a **Remote Target**, ensure **Discover network targets** is checked, then click **Configure** to add the IP address and port of the device where the application is running.*

Click the Target’s **inspect** link to launch DevTools. It will be immediately familiar to anyone with browser debugging experience.

![Chrome DevTools](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523888chrome-devtools.png)

The **+ Add folder to workspace** link allows you to select where the Node.js files are located on your system, so it becomes easier to load other modules and make changes.

Clicking any line number sets a breakpoint, denoted by a green marker, which stops execution when that code is reached:

![Chrome DevTools breakpoint](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523886chrome-breakpoint.png)

Variables can be added to the **Watch** pane on the right by clicking the **+** icon and entering their name. Their value is shown whenever execution is paused.

The **Call Stack** pane shows which functions were called to reach this point.



The **Scope** pane shows the state of all available local and global variables.

The **Breakpoints** pane shows a list of all breakpoints and allows them to be enabled or disabled.

The icons above the **Debugger paused** message can be used to resume execution, step over, step into, step out, step through, deactivate all breakpoints, and pause on exceptions.

## Node.js Debugging with VS Code

VS Code Node.js debugging can be launched without any configuration when you’re running a Node.js application on your local system. Open the starting file (typically `index.js`), activate the **Run and Debug** pane, and click the **Run and Debug Node.js (F5)** button.

![VS Code debugger](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523892vscode-debug.png)

The debugging screen is similar to Chrome DevTools with a **Variables**, **Watch**, **Call stack**, **Loaded scripts**, and **Breakpoints** list.

![VS Code breakpoint](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523890vscode-breakpoint.png)

A breakpoint can be set by clicking the gutter next to the line number. You can also right-click.

![VS Code breakpoint options](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523897vscode-points.png)

With this right-click, you can set the following:

1. A standard breakpoint.

2. A conditional breakpoint which stops when criteria are met — for example, `count > 3`.

3. A logpoint, which is effectively `console.log()` without code! Any string can be entered with expressions denoted in curly braces — for example, `{count}` to display the value of the `count` variable.

   

   ![VS Code logpoint](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523896vscode-logpoint.png)

*Note: don’t forget to hit Return for VS Code to create your conditional breakpoint or logpoint.*

The debugging icon bar at the top can be used to resume execution, step over, step into, step out, restart, or stop the application and debugging. Identical options are also available from the **Debug** item in the menu.

For more information, refer to [Debugging in Visual Studio Code](https://code.visualstudio.com/docs/introvideos/debugging).

### Advanced Debugging Configuration

Further configuration is required when you’re debugging a remote service or need to use different launch options. VS Code stores launch configurations in a `launch.json` file generated inside the `.vscode` folder within your project. To generate or edit the file, click the cog icon at the top right of the **Run and Debug** pane.

![VS Code launch configuration](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523893vscode-launch.png)

Any number of configuration settings can be added to the `configurations` array. Click the **Add Configuration** button to choose an option. VS Code can either:

1. **launch** a process using Node.js itself, or
2. **attach** to a Node.js inspector process, perhaps running on a remote machine or Docker container

In the example above, a single Nodemon launch configuration has been defined. Save `launch.json`, select `nodemon` from the drop-down list at the top of the **Run and Debug** pane, and click the green start icon.

![VS Code launch](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2020/02/1581523895vscode-launchconfig.png)

For further information, see [VS Code Launch configurations](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations).

## Other Node.js Debugging Tools

The [Node.js Debugging Guide](https://nodejs.org/en/docs/guides/debugging-getting-started/) provides advice for other IDEs and editors including Visual Studio, JetBrains, WebStorm, Gitpod, and Eclipse. Atom also has a [node-debug](https://atom.io/packages/node-debug) extension.

[ndb](https://github.com/GoogleChromeLabs/ndb) offers an *improved debugging experience* with powerful features such as attaching to child processes and script blackboxing so only code in specific folders is shown.

The [IBM report-toolkit for Node.js](https://github.com/ibm/report-toolkit) works by analyzing data output when `node` is run with the `--experimental-report` option.

Finally, commercial services such as [LogRocket](https://logrocket.com/) and [Sentry.io](https://sentry.io/) integrate with your live web application in both the client and the server to record errors as they’re encountered by users.

## Get Debugging!

Node.js has a range of great debugging tools and code analyzers which can improve the speed and reliability of your application. Whether or not they can tempt you away from `console.log()` is another matter!

[![img](https://secure.gravatar.com/avatar/439aeaff7de2bae365adc3eb4947b44d?s=96&d=mm&r=g)](https://www.sitepoint.com/author/craig-buckler/)










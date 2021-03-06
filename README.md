
需解决的问题
------
### 前端代码可复用性不强，重复造汽车的问题
在实现签约，续约，解约功能的时候，有一个需求是要把部分独家作品改成非独家，需要把用户所有的图片分页显示，分页跳转无刷新，并能点击图片选择
是独家作品还是非独家作品。
<br />
	思路一：因为这是一个比较常见的组件，我们公司也是有很多代码积累的，这样的组件应该是有。“猴哥，这个无刷新分页功能以前应该实现过，我想拿来
	直接用”，猴哥说：以前好像一个页面上好像有用过，但是跟需求功能上有点差异，没有封装，你可以自己写一个。
<br />  
	思路二：为了提高工作效率，在互联网上找相应的插件。比如：jPages，jPaginate等。因为这是一个小功能，而插件考虑的面比较全，代码臃肿。而且我也
	有自己需求定制，还需要看懂作者源码，修改UI，定制功能，最好作者还要保证插件代码没问题。此外，这样导致代码量较大，维护比较困难。那
	就自己写吧。
<br />
	思路三：自己写无刷新分页功能。因为开发周期的问题，首要的任务是保证功能没问题，其次是开发时间的长短，最后才考虑到性能，优化，耦合等问题。
	往往最终的结果是：以后这个组件不会去维护了。因为功能完善之后，运营测试通过，发布外网，比较稳定了。如果你要把原有稳定的代码换成新
	开发的组件，相当于把之前的工作重新做一遍，重新测试，担心交互问题......

### 担心的问题
	1 下次在来一个需求，也是需要无刷新分页，但是没有了图片选择功能，页面布局也不一样了......如果把原来的js引用过来，担心全局变量污染问题，dom
	绑定问题（原来的写法和dom耦合太多），如果新增功能，怕影响原来的页面。我是不是要重新copy一份，把它放在当前需求的作用域内，这样我才放心...
	<br />
	2 原来的分页功能有一些小问题，可是这个代码又复制了好多分，每一处我都需要更改...
	<br />
	3 这个组件的性能问题基本没有考虑，没有其余的时间去维护了，只能任其自生自灭...
	<br />
	4 公司下次再来一个人，又是一个分页无刷新组件...这样我们的js库里就出现了多份功能相似的分页无刷新组件。无法维护
	同样的问题出现在表格组件，图片处理组件，表情组件，图片轮播组件，弹出框组件。甚至是输入框，输入框按钮，表格配色都会不统一，网站风格也因为
	差异太多，风格不统一。

### 前后端交互的问题
	功能编写前，前后端统一接口，以注释的形式写在源码中，针对接口开发。出现问题时候，前端检查自己的接口是否正确。
	参数的传递统一为json（2个参数以上）
								
### 代码优化的问题
	1 js都是写在页面头部，影响整体网页的加载速度
	2 js，css没有相应的压缩机制，浪费网络资源
	3 代码写法上的改进，比如jq绑定事件写法，寻找dom方式......
	4 前端测试


由于经验和能力上的不足，仅能提供一个简略的方案，具体实施进展实时更新到git。此项目在github开源，尽可能的和社区互动。
------

### 解决的思路
以jq为基础库，汇图、昵图已有的js资源，开发者自己掌握的类库为基础，借鉴，结合开源社区优秀的类库，构建一个适合汇图前端组件UI库，并提供完整
的demo和api

### 兼容性
ie6+， ff，chrome

### 使用范围
汇图、汇图后台、昵图2.0、昵图2.0后台新开发的功能；
重复太多，有影响使用的原有功能；
其它项目的快速开发

### 开发周期
初步毫无根据地预算：1年（如果2个前端开发）完成基本架构和初步demo用例

### 开发时间
在完成汇图，昵图日常开发和维护任务的前提下

### 希望达到的要求
	1 功能稳定，性能优化
	2 兼容性良好
	3 维护简单，尽量保持ui界面和逻辑代码的解耦（方便替换ui）

### 项目的目的
	1 解决上述存在的问题，方便管理，方便维护，尽量避免重复造车现象
	2 开发人员更多的时间花在类库的维护，性能的优化上，而不是日复一日的依据需求带有很多重复性工作的编码
	3 将开源社区的一些先进思想和类库(比如require.js, backbone.js......)运用到实际项目中，给公司创造更大的价值，提升开发的业务水平

### 需要的资源
一个可以做测试和展示用的空间，提供外网访问





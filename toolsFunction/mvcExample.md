https://stackedit.io/#

前端MVC示例剖析
=====================

先展示一个基于MVC书写的示例。
参考网站：[http://developer.51cto.com/art/201212/372725.htm][1]

----------


代码如下：
---------

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
    	<meta http-equiv="Content-Type" content="text/html;charset=UTF-8" />
    	<title></title>
    	 <script src="http://skin.huitu.com/js/jquery.js" type="text/javascript"></script>
    	<script style="text/javascript">
        /**  
         * 模型。  
         *  
         * 模型存储所有元素，并在状态变更时通知观察者（Observer）。  
         */   
        function ListModel(items) {  
            this._items = items;        // 所有元素  
            this._selectedIndex = -1;   // 被选择元素的索引  
         
            this.itemAdded = new Event(this);  
            this.itemRemoved = new Event(this);  
            this.selectedIndexChanged = new Event(this);  
        }  
         
        ListModel.prototype = {  
            getItems : function () {  
                return [].concat(this._items);  
            },  
         
            addItem : function (item) {  
                this._items.push(item);  
                this.itemAdded.notify({item : item});  
            },  
         
            removeItemAt : function (index) {  
                var item;  
         
                item = this._items[index];  
                this._items.splice(index, 1);  
                this.itemRemoved.notify({item : item});  
         
                if (index === this._selectedIndex) {  
                    this.setSelectedIndex(-1);  
                }  
            },  
         
            getSelectedIndex : function () {  
                return this._selectedIndex;  
            },  
         
            setSelectedIndex : function (index) {  
                var previousIndex;  
         
                previousIndex = this._selectedIndex;  
                this._selectedIndex = index;  
                this.selectedIndexChanged.notify({previous : previousIndex});  
            }  
        };  
    
    	<!--Event 是一个简单的实现了观察者模式（Observer pattern）的类：-->
    
        function Event(sender) {  
            this._sender = sender;  
            this._listeners = [];  
        }  
         
        Event.prototype = {  
            attach : function (listener) {  
                this._listeners.push(listener);  
            },  
         
            notify : function (args) {  
                var index;  
         
                for (index = 0; index < this._listeners.length; index += 1) {  
                    this._listeners[index](this._sender, args);  
                }  
            }  
        };  
    	</script>
    	<script style="text/javascript">
        /**  
         * 视图。  
         *   
         * 视图显示模型数据，并触发 UI 事件。  
         * 控制器用来处理这些用户交互事件  
         */   
        function ListView(model, elements) {  
            this._model = model;  
            this._elements = elements;  
         
            this.listModified = new Event(this);  
            this.addButtonClicked = new Event(this);  
            this.delButtonClicked = new Event(this);  
         
            var _this = this;  
         
            // 绑定模型监听器  
            this._model.itemAdded.attach(function () {  
                _this.rebuildList();  
            });  
         
            this._model.itemRemoved.attach(function () {  
                _this.rebuildList();  
            });  
         
            // 将监听器绑定到 HTML 控件上  
            this._elements.list.change(function (e) {  
                _this.listModified.notify({ index : e.target.selectedIndex });  
            });  
         
            this._elements.addButton.click(function () {  
                _this.addButtonClicked.notify();  
            });  
         
            this._elements.delButton.click(function () {  
                _this.delButtonClicked.notify();  
            });  
        }  
         
        ListView.prototype = {  
            show : function () {  
                this.rebuildList();  
            },  
         
            rebuildList : function () {  
                var list, items, key;  
         
                list = this._elements.list;  
                list.html('');  
         
                items = this._model.getItems();  
                for (key in items) {  
                    if (items.hasOwnProperty(key)) {  
                        list.append($('<option>' + items[key] + '</option>'));  
                    }  
                }  
         
                this._model.setSelectedIndex(-1);  
            }  
        };  
         
        /**  
         * 控制器。  
         *  
         * 控制器响应用户操作，调用模型上的变化函数。  
         */   
        function ListController(model, view) {  
            this._model = model;  
            this._view = view;  
         
            var _this = this;  
         
            this._view.listModified.attach(function (sender, args) {  
                _this.updateSelected(args.index);  
            });  
         
            this._view.addButtonClicked.attach(function () {  
                _this.addItem();  
            });  
         
            this._view.delButtonClicked.attach(function () {  
                _this.delItem();  
            });  
        }  
         
        ListController.prototype = {  
            addItem : function () {  
                var item = window.prompt('Add item:', '');  
                if (item) {  
                    this._model.addItem(item);  
                }  
            },  
         
            delItem : function () {  
                var index;  
         
                index = this._model.getSelectedIndex();  
                if (index !== -1) {  
                    this._model.removeItemAt(this._model.getSelectedIndex());  
                }  
            },  
         
            updateSelected : function (index) {  
                this._model.setSelectedIndex(index);  
            }  
        };  
    	</script>
    	<script type="text/javascript">
    		$(function () {  
    	        var model = new ListModel(['PHP', 'JavaScript']),  
    	     
    	        view = new ListView(model, {  
    	            'list' : $('#list'),   
    	            'addButton' : $('#plusBtn'),   
    	            'delButton' : $('#minusBtn')  
    	        }),  
    	     
    	        controller = new ListController(model, view);          
    	        view.show();  
    	    });  
    </script>
    </head>
    <body>
    	    
         
        <select id="list" size="10" style="width: 15em"></select><br/>  
        <button id="plusBtn">  +  </button>  
        <button id="minusBtn">  -  </button>  
    </body>
    </html>

### **先弄清几个重要的概念**

 

 - **面向对象**
 

> 先来看看wiki中的定义：[http://zh.wikipedia.org/wiki/面向对象][2]
  我们简单概述：    
  

    1 面向对象（Object-oriented)由面向对象程序设计（Object-oriented programming）和面向对象的分析（Object Orient Analysis）方法构成。
    
    2 面向对象的分析方法是利用面向对象的信息建模概念，如实体、关系、属性等，同时运用封装、继承、多态等机制来构造模拟现实系统的方法。
    
    3 在面向对象的设计中，初始元素是对象，具有共同特征的对象归纳成一类，组织类之间的等级关系，构造类库。

    由此：类，对象，属性，行为等一堆名词构成面向对象的全部。继承，多态，封装等只是处理这些名词时所需的一些技巧方法。所以在OO时，不防先构建一个类图，将类，行为，属性之间的的关系在OOP前明了。

 - **MVC**

   

>  MVC是OO的一种经典模式，一种“规范”，通过这种规范，你可以更好的处理那些名词之间的关系。
   这样一种既定的规范，有助于我们整理出那一堆名词之间的关系。
   ![enter image description here][3]
> 
> 
> M 程序员编写程序应有的功能（实现算法等等）、数据库专家进行数据管理和数据库设计(可以实现具体的功能)。
V 界面设计人员进行图形界面设计。
C 负责转发请求，对请求进行处理。

回到这个js中



    


  


  [1]: http://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1
  [2]: http://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1
  [3]: //upload.wikimedia.org/wikipedia/commons/f/f0/ModelViewControllerDiagramZh.png
### vue-sxl
## 基于vue1.0.12构建的多页面模板
=======================================
 * 单个文件打包单个文件的引用
 * 没有使用路由
 * 使用多页面开发

 ##使用

  1. 下载解压
  2. 使用 npm/cnpm install
  3. 开发环境热更新 npm run dev
  4. 正式环境打包 npm run build
  5. -------->后续更多
  
-----------------------------------------
###Vue语法
-----------------------------------------

###vue 组件的生命周期组成
-----------------------------------------
1.beforeCreate : 组件实例刚被创建，组件属性计算之前（data等）；
2.create: 组件实例创建完成，属性已经绑定，但DOM未生成，$el属性还不存在
3.beforeMount: 模板编译/挂载之前
4.mounted:模板编译/挂载之后
5.beforeUpdate:组件更新之前
6.updated: 组件更新之后
7.activated: for keep-alive 组件被激活是调用
8.deactivated: for keep-alive 组件被移除时调用
9.beforeDestory:组件销毁之前
10.destroyed:组件销毁之后

-----------------------------------------
###模板语法

    v-once: 节点上的所有绑定，单次插值，之后数据不再变化
    v-html:{{}}解析为纯文本，html输出为标签，但数据绑定被忽略，标签中的内容也会被忽略
    v-bind: 对应于html属性，{{}}不能用于此
            除了变量，还可以用表达式，每个绑定只能包含单个表达式（三目运算也可以），不要试图访问用户在模板表达式中定义的全局变量
    filters: 过滤器

    #缩写：
    <a v-bind:href="url"></a> ->  <a :href="url"></a>
    <a v-on:click="doSomeThing"> -> <a @click="doSomeThing"></a>

------------------------------------------------------------------

##计算属性
    
    #表达式：
        {{ message.split('').reverse().join('') }}

        计算属性也可以当做一种属性，依赖于其他属性

        computed: {
            //计算属性的getter方法
            reverseMessage: function() {
                return this.message.spit('').reverse().join('')
            }
        }

        计算属性的getter方法干净无副作用

    #函数：
        <p>反转语句：{{ reverseMessage() }} <\/p>

        methods: {
            //计算属性的getter方法
            reverseMessage: function() {
            return this.message.split('').reverse().join('')
            }
        }

    两种方式得到的结果是一样的，不同的是计算属性是基于其依赖的缓存

------
    #计算属性 vs $WATCH

        $watch 用于监听已有的普通属性，若是沿用上面的例子，那么思路就是指定message以及reversedMessage两个属性，之后监听message属性的变化的同时，改变reversedMessage的值。 
        但初始化时必须将reversedMessage手动设置为message的倒序字符串。 
        上面例子似乎不妥，所以换用下面的例子：

        data: {
         firstName: 'aaa',
         lastName: 'bbb',
         fullName: 'aaabbb'
        },
        computed: {
         computedFullName: function () {
           return this.firstName + this.lastName;
         }
        },
        watch: {
         firstName: function (val) {
           this.fullName = val + ' ' + this.lastName
         },
         lastName: function (val) {
           this.fullName = this.firstName + ' ' + val
         }
        }

        无论是获取computedFullName还是获取fullName的值都是一样的。但明显计算属性的代码更简洁精炼。
--------

    ##计算属性set方法：
        <p> 完整的名字： {{ fullName }} </p>

        computed: {
            fullName:{
                //getter
                get: function() {
                    return this.firstName + '' + this.lastName
                },
                //setter
                set: function() {
                    var names = newValue.split('')
                    this.firstName = names[0]
                    this.lastName = names[names.length - 1]
                }
            }
        }

---------------------------------------------------------------

###标签的Class属性

    ##class是一个属性，可以用V-bind来指定，普通的方式要求使用者自己控制字符串，但字符串的拼接麻烦，

    #传递一个对象
      通过对象某个键值对应的bollean类型值，来控制class的显隐

        <div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"
        </div>
        data: {
            isActive: true,
            haError: false
        }
        或者

        <div class="static" v-bind:class="classObject"></div>
        data: {
            classObject: {
                active: true,
                'text-danger': false
            }
        }

        或者

        data {
            isActive: true,
            err:null
        },
        computed: {
            classObject: function() {
                return {
                    active: this.isActive && !this.error,
                    'text-danger': this.error && this.error.type == "fatal"
                }
            }
        }
        ------
        最终渲染的结构为 <div class="static active"></div>

    ##总结 两种类型，传递对象的需要通过true或者false来控制显示，如果频繁的去除添加某种属性时非常适用；传递数组是通过直接传值显示，如果是多种属性频繁切换，那么这种情况更适合列表而不是对象。

###标签的内嵌样式    

    * 可以传递一个对象，与class属性传值传递对象的方式一样，直接传递或者是传递一个对象类型的属性或者是计算属性
    * 此时的对象key为属性名，值为属性值。
    * 属性名字可以为驼峰式 camelCase;或是分割式 kabab-case
    * 同样可以传递数组，数组中每个元素都是一个对象，包含一个或多个样式key, value的对象
    * 自动添加前缀

###条件渲染
    ##概览
    v-if: 值为一个属性，或是一个表达式
    v-show: 与v-if类似，只是此 v-show 会始终渲染并保持在dom中，简单切换display属性；除此之外，这个不支持template语法，即包含多个标签
    v-else: 与v-if或v-show配合使用

    #简单示例
    <h1 v-if="ok">ok</h1>
    <h1 v-else>NO</h1>
    #多个元素
    因为v-if必须放在一个元素里，所以一次性只能控制一个元素及其子元素，若要同时控制多个元素：
    <template v-if="ok">
        <h1>Title</h1>
        <p>Paragrph 1</p>
    </template>
    <template>
        <!--略-->
    </template>

    #多层if 最后一个else
        有时候有多个条件，则可以嵌套进行if-else

        <div v-if = ""></div>
        <template v-else>
            <div v-if=""></div>
            <div v-else></div>
        </template>

        ##  v-if 是真实的条件渲染，因为它会确保条件块在切换当中适当地销毁与重建条件块内的事件监听器和子组件。 
        v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——在条件第一次变为真时才开始局部编译（编译会被缓存起来）。 
        相比之下，
         v-show 简单得多——元素始终被编译并保留，只是简单地基于 CSS 切换。 
        一般来说， v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。因此，如果需要频繁切换使用 v-show 较好，如果在运行时条件不大可能改变则使用 v-if 较好。

###列表渲染
    ##概览
    v-for: 让某个标签元素循环出现，一般值都是表达式：item in items 这种形式出现

    ##基本示例

        <ul>
            <li v-for = "item in items">item.text</li>
        <ul>

        data: {
            items: [
                {text: 'foo'},
                {
                 text: 'bar'
                }
            ]
        }

        #若干要在循环过程中使用index,可以这样
        <li v-for = "(item, index) in items"> item.text</li>
        #遍历列表
            >与条件判断类似， v-for 也有 template特性
            <ul>
                <template v-for='item in items'>
                    <li>{{item.msg}}</li>
                    <li class='divider'></li>
                </template>
            </ul>
        #遍历对象
            >除了遍历列表（数组）中的元素，也可以遍历对象中的键值对，遍历对象时，先值，后键，最后索引

            <li v-for="(value,key,index) in objectItem"></li>
        #遍历整数
            <li v-for='item in 10'></li>
            结果是[1,10]遍历
    ##列表操作重点
        1，直接操作，不会引起视图变化
            >vm.items[indexOfItem] = newValue
           需要运用Vue自带的set方法：
            >Vue.set(example.items, indexOfItem, newValue)
        2.想要直接修改列表长度属性来改变列表内容长度，也是  不可取的，如下
            >vm.items.length = newLength
            >example.items.splic(indexOfItem, 1, newValue)
         正确的使用splice方法：
           example.items.splice(newLength)

        ##可以改变视图的方法很多：push(), pop(), shift(), unshift(), splice(), sort(), reverse()等

1. 你是否尝试过对数组元素进行排序？

JavaScript默认使用字典序(alphanumeric)来排序。因此， [1,2,5,10].sort() 的结果是 [1, 10, 2, 5] 。
如果你想正确的排序，应该这样做： [1,2,5,10].sort((a, b) => a - b)
2. new Date() 十分好用

new Date() 的使用方法有：
不接收任何参数：返回当前时间；
接收一个参数 x : 返回1970年1月1日 + x 毫秒的值。
new Date(1, 1, 1) 返回1901年2月1号。
然而....， new Date(2016, 1, 1) 不会在1900年的基础上加2016，而只是表示2016年。
3. 替换函数没有真的替换？

let s = "bob"
  const replaced = s.replace('b', 'l')
  replaced === "lob" // 只会替换掉第一个b
  s === "bob" // 并且s的值不会变
如果你想把所有的b都替换掉，要使用正则：
"bob".replace(/b/g, 'l') === 'lol'
4. 谨慎对待比较运算

// 这些可以
  'abc' === 'abc' // true
  1 === 1         // true
  // 然而这些不行
  [1,2,3] === [1,2,3] // false
  {a: 1} === {a: 1}   // false
  {} === {}           // false
因为[1,2,3]和[1,2,3]是两个不同的数组，只是它们的元素碰巧相同。因此，不能简单的通过 === 来判断。
5. 数组不是基础类型

typeof {} === 'object'  // true
  typeof 'a' === 'string' // true
  typeof 1 === number     // true
  // 但是....
  typeof [] === 'object'  // true
如果要判断一个变量 var 是否是数组，你需要使用 Array.isArray(var) 。
6. 闭包

这是一个经典的JavaScript面试题：
const Greeters = []
  for (var i = 0 ; i < 10 ; i++) {
    Greeters.push(function () { return console.log(i) })
  }
  Greeters[0]() // 10
  Greeters[1]() // 10
  Greeters[2]() // 10
虽然期望输出0,1,2,...，然而实际上却不会。知道如何Debug嘛？
有两种方法：
使用 let 而不是 var 。备注：可以参考Fundebug的另一篇博客 ES6之"let"能替代"var"吗?
使用 bind 函数。备注：可以参考Fundebug的另一篇博客 JavaScript初学者必看“this”
Greeters.push(console.log.bind(null, i))
当然，还有很多解法。这两种是我最喜欢的！
7. 关于 bind

下面这段代码会输出什么结果？
class Foo {
    constructor (name) {
      this.name = name
    }
    greet () {
      console.log('hello, this is ', this.name)
    }
    someThingAsync () {
      return Promise.resolve()
    }
    asyncGreet () {
      this.someThingAsync()
      .then(this.greet)
    }
  }
  new Foo('dog').asyncGreet()
如果你说程序会崩溃，并且报错：Cannot read property 'name' of undefined。
因为第16行的 geet 没有在正确的环境下执行。当然，也有很多方法解决这个BUG！
我喜欢使用 bind 函数来解决问题：
asyncGreet () {
  this.someThingAsync()
  .then(this.greet.bind(this))
}
这样会确保 greet 会被Foo的实例调用，而不是局部的函数的 this 。
如果你想要 greet 永远不会绑定到错误的作用域，你可以在构造函数里面使用 bind 来绑 。
class Foo {
  constructor (name) {
    this.name = name
    this.greet = this.greet.bind(this)
  }
}
你也可以使用箭头函数(=>)来防止作用域被修改。备注：可以参考Fundebug的另一篇博客 JavaScript初学者必看“箭头函数” 。
asyncGreet () {
  this.someThingAsync()
  .then(() => {
    this.greet()
  })
}
8. Math.min()比Math.max()大

Math.min() < Math.max() // false
因为Math.min() 返回 Infinity, 而 Math.max()返回 -Infinity。
# Symbol

+ 一种新的原始数据类型  `undefined`  `null`  `boolean`  `string`  `number`  `object`  `symbol`

+ 通过函数生成

  > ```javascript
  > let s = Symbol();
  > 
  > typeof s 'symbol'
  > ```

+ 不是构造函数，无法使用 `new`



### 使用

+ 可以接受一个字符串为参数，作为对Symbol实例的描述

  > ```javascript
  > let s1 = Symbol('foo');
  > let s2 = Symbol('bar');
  > 
  > s1	//	Symbol(foo)
  > s2	//	Symbol(bar)
  > 
  > s1.toString()	//	'Symbol(foo)'
  > s2.toString()	//	'Symbol(bar)'
  > 
  > ```

+ 参数只是描述，因此参数相同的Symbol对象并不相等

  > ```javascript
  > let s1 = Symbol();
  > let s2 = Symbol();
  > s1 == s2 		// false
  > 
  > let s1 = Symbol('foo');
  > let s2 = Symbol('foo');
  > 
  > s1 == s2		//	false
  > ```

+ Symbol值不能和其他类型值进行运算，会报错

+ Symbol.for()函数  接收一个字符串作为参数，然后搜索有没有以这个参数为名称的Symbol值，有的话返回这个值，没有的话创建这个值。

  > ```javascript
  > let s1 = Symbol.for('foo');
  > let s2 = Symbol.for('foo');
  > 
  > s1 === s2	//	true
  > ```

+ Symbol.keyFor()  返回Symbol的key


## Let

+ 变量只在let对应的代码块中生效

  > ```javascript
  > {
  >     	let a = 10;
  >     	var b = 1;
  > }
  > a	// 无法访问
  > b	//	1
  > ```

+ 不存在变量提升

  > ```javascript
  > console.log(foo) //	undefined
  > var foo = 1;
  > 
  > console.log(too)	//	报错
  > let too = 1;
  > ```

+ 暂时性死区（声名之前不可用）

  > 块级作用域存在 let 命令，它声明的变量就 “绑定” 这个区域，不再受外部的影响。
  >
  > ```javascript
  > var temp = 123;
  > 
  > if(true){
  >     temp = 'abc';	//	报错
  >     let temp;
  > }
  > ```
  >
  
+ 不允许重复声明

  > ```javascript
  > function f(){
  >     var a = 1;
  >     let a = 2; //	报错
  > }
  > ```



## const

+ 声明常量

  > ```javascript
  > const a = 1;
  > 
  > a = 2; 	// 	报错
  > 
  > const foo;	//	不立即赋值也会报错
  > ```

+ 不一定不可变

  > const 保证的是变量指向的那个**内存地址所保存的数据**不可改动。
  >
  > 对于对象类型的数据 const指向的仅仅是一个指针，因此对对象的修改都是可以的
  >
  > ```javascript
  > const obj = {};
  > obj.a = 1;	//	可以
  > obj.b = 2; 	//	可以
  > 
  > obj = {}	//	不可以  修改了指针
  > ```

+ 如何实现定义一个常量对象

  > ```javascript
  > const foo = Object.freeze({})
  > ```


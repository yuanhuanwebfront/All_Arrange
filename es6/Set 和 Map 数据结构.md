# Set 和 Map 数据结构



## Set

+ **基本用法**

  > ```javascript
  > const s = new Set();
  > 
  > [2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
  > 
  > s;	//	[2, 3, 5, 4]
  > ```

+ **接受数组作为参数**

  > ```javascript
  > const set = new Set([1, 2, 3, 4, 4]);
  > [...set]	//	[1, 2, 3, 4]
  > ```

+ **属性及方法**

  + `size`  属性

    ```javascript
    let set = new Set([1, 2, 4, 4]);
    set.size;		// 3
    ```

  + `add`  函数

    ```javascript
    let set = new Set();
    let s = set.add(1);
    s; //	为set本身。  add函数的返回值是set实例本身
    ```

  + `delete`  函数

    ```javascript
    let s = new Set([1, 2]);
    
    let isSuccessDel = s.delete(1);		//	返回值是布尔值  标识是否删除成功
    ```

  + `has`  函数

    ```javascript
    let set = new Set([1, 2])
    
    let hasItem = set.has(1)		//	返回布尔值  标识set中是否有这个值
    ```

  + `clear`  函数   清空set

    ```javascript
    let set = new Set([1, 2, 3]);
    
    set.clear();		//	没有返回值
    
    set;	//	[]  
    ```

+ **Set遍历方法**

  + `keys`   `values`   返回键名和键值

    ```javascript
    //	由于set结构没有键名 只有键值 所以keys和values的行为完全一致。
    let set = new Set(['red', 'green', 'blue'])
    
    for(let item of set.keys()){
      console.log(item);	//	red、green、blue
    }
    
    let set = new Set(['red', 'green', 'blue'])
    
    for(let item of set.values()){
      console.log(item);	//	red、green、blue
    }
    
    ```
  
  + `entries` 方法  返回键名和键值的数组
  
    ```javascript
    let set = new Set(['red', 'green', 'blue'])
    
    for(let item of set.entries()){
      console.log(item) 		//	['red', 'red']  ['green', 'green']
    }
    ```
  
  + `forEach` 方法，和数组一样。
  
    ```javascript
    let set = new Set(['red', 'green', 'blue']);
    
    set.forEach((value, key) => {
      console.log(`${key} : ${value}`);		// red:red   green:green   blue:blue
    })
    ```



##	WeakSet

+ WeakSet的成员只能是`对象`

+ 属性及方法

  + `add`方法   用于添加一个新成员

    ```javascript
    const ws = new WeakSet();
    ws.add(window);		//	返回这个WeakSet本身
    ```

  + `delete` 方法，用于删除一个成员

    ```javascript
    const ws = new WeakSet();
    ws.add(Window);
    ws.delete(window);	//	返回布尔值
    ```

  + `has`  方法  用于判断WeakSet中是否有该元素

    ```javascript
    const ws = new WeakSet();
    ws.add(window);
    ws.has(window);				//	true
    ```



## Map

+ Map是键值可以是各种类型的对象。比如，Object结构提供了`字符串-值`的对应，那么Map提供的是`值-值`的对应。

+ **属性和方法**

  + `size `  属性。    返回Map结构的成员总数

    ```javascript
    const map = new Map();
    map.set('foo', true);
    map.set('bar', false);
    
    map.size;			//	2
    ```

  + `set` 方法。    用于设置`key` 对应的`value`，返回最新的map结构。

    ```javascript
    const m = new Map();
    
    m.set('edition', 6);
    m.set(262, 'stand');
    m.set(undefined, 'haha1')
    //	因为返回原结构。可以使用链式写法
    m.set('1', 2).set(2, 3)
    ```

  + `get`方法。 获取`key` 对应的键值

    ```javascript
    const m = new Map();
    
    m.set('a', 1);
    
    m.get('a')		//	1
    ```

  + `has`方法    表示某个键是否在当前Map对象之中。

    ```javascript
    const m = new Map();
    m.set('aa', 1);
    m.has('aa')		//	true
    ```

  + `delete` 方法    删除某个key及其对应的值

    ```javascript
    const m = new Map();
    const a = {};
    m.set(a, 1);
    m.delete(a);		//	true
    ```

  + `clear`  方法   清空Map



## WeakMap

+ WeakMap不同于Map的点是，WeakMap的键值只能是对象

  ```javascript
  const m = new WeakMap();
  
  m.set(1, 2);  m.set('a', 3);	m.set(null, 4);		//	都会报错
  ```

+ 使用场合：键对应的对象可能会在将来消失，弱引用的只是键名，而不是键值。

  ```javascript
  const wm = new WeakMap();
  let key = {};
  let obj = {foo: 1};
  
  wm.set(key, obj);
  obj = null
  wm.get(key);	//	{foo: 1}
  ```

+ 方法

  + 因为某个键名可能会突然消失，因此无法遍历和获取Size;
  + 支持  `get` 、 `set`  方法。
  + 支持  `has` 、 `delete`  方法。




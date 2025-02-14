---
title: JS常用的求交集、并集、差集的方法
i18nReady: false
---

<!-- # JS常用的求交集、并集、差集的方法 -->

## 普通数组

> 示例数组

```js
let a = [1, 2, 3];
let b = [4, 3, 2];
```

### 一、并集(A∪B)

```js
//方法一：
;
// 虽然 NaN 和 NaN 不相等，但是在 Set 集合里面只会存在一个
// undefined 和 Infinity 在 Set 集合里面也只会存在一个
let a = [1, 2, 3, NaN];
let b = [2, 4, 5, NaN];
const union  =  (arr1,arr2) => [...new Set([...arr1, ...arr2])]// [1, 2, 3, NaN, 4, 5]

//方法二：
const union = (arr1,arr2) =>  Array.from(new Set(arr1.concat(arr2)))

//先b筛选a中没有的，再连接数组
const union = (arr1,arr2) => arr1.concat(arr2.filter(val => !arr1.includes(val)))
const union = (arr1,arr2) => arr1.concat(arr2.filter(val=> arr2.indexOf(val) === -1));
```



### 二、交集（*A*∩B）

```js
let intersection  = (arr1,arr2) => [...new Set(arr1.filter(val => arr2.includes(val)))];//[2, NaN]
```

### 三、差集(A-B)

```js
// 差集（a 相对于 b 的差集） 属于a不属于b的元素
let difference  = (arr1,arr2) => [...new Set(arr1.filter(val => !arr2.includes(val)))];
 
//返回a与b数组区别的集合（交集取反） 
let a = [1, 2, 3];
let b = [2, 4, 5];
let difference = a.concat(b).filter(v => !a.includes(v) || !b.includes(v));
let difference = a.concat(b).filter(v => !(a.includes(v) && b.includes(v)));
console.log(difference)// [1,3,4,5]

//方法二：先转为 Set
let aSet = new Set(a);
let bSet = new Set(b)
//  
let difference = Array.from(new Set(a.concat(b).filter(v => !aSet.has(v) || !bSet.has(v))));
console.log(difference) // [1,3,4,5]

```


## 对象数组

> 示例数组

```js
let a=[
    {id:'01',name:'product01'},
    {id:'02',name:'product02'},
    {id:'03',name:'product03'},
    {id:'04',name:'product04'},
    {id:'05',name:'product05'}
    ];
let b=[
    {id:'03',name:'product03'},
    {id:'06',name:'product06'},
    {id:'07',name:'product07'},
    {id:'08',name:'product08'},

];

```

### 一、并集（A∪B）

```js
// 用额外对象记录当前项 id 相同的是否收集，此 id 未收集，则进行收集
//写法一：
const union = (arr1,arr2) =>{
    let obj = {};
    let arr = arr1.concat(arr2);
   return arr.reduce( (pre,cur) => {
        if(!obj[cur.id]){
            pre.push(cur)
            obj[cur.id] = true
        }
        return pre
    },[])
}

//写法二：
const union = (arr1,arr2)=>{
  let arr = arr1.concat(arr2)
  let res = []
  for(let i = 0; i < arr.length; i++){
    if(res.findIndex(item => item.id === arr[i].id)===-1){
      res.push(c[i])
    }
  }
  return res
}
  //简化
  //判断目标在数组中相同的 id 是否存在
const isExist = (arr,target,attr) => arr.findIndex(item => item[attr] == target[attr]) != -1 
//合并数组并遍历，判断新数组不存在当前项，则收集
const union = (arr1,arr2) =>  arr1.concat(arr2).reduce((pre,cur) => isExist(pre,cur,'id') ? pre  : [...pre,cur],[])
```


### 二、交集（*A*∩B）


```js
//方法一：
const intersection = (arr1,arr2) => {
    const aids = a.map(item => item.id)
    return arr2.filter(item => aids.includes(item.id))
}
//方法二：
const intersection = (arr1,arr2) =>  arr1.reduce((pre,cur) => arr2.findIndex(item => item.id == cur.id) != -1 ? [...pre,cur] : pre,[])
```

### 三、差集(A-B)

```js
//找出arr1数组中，arr2数组没有的对象
 const diff = (arr1, arr2) =>  arr1.filter(i => arr2.every(j => i.id !== j.id))
 const diff = (arr1, arr2) =>  arr1.filter(i => !arr2.some(j => i.id === j.id))
```
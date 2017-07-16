# 在线编程题

## 1.查找数组元素位置
```
function indexOf(arr, item) {
  if (Array.prototype.indexOf){
      return arr.indexOf(item);
  } else {
      for (var i = 0; i < arr.length; i++){
          if (arr[i] === item){
              return i;
          }
      }
  }     
  return -1;
}
```
## 2.数组求和
### 不考虑算法复杂度，用递归做：
```
function sum(arr) {
    var len = arr.length;
    if(len == 0){
        return 0;
    } else if (len == 1){
        return arr[0];
    } else {
        return arr[0] + sum(arr.slice(1));
    }
}
```
### 常规循环：
```
function sum(arr) {
    var s = 0;
    for (var i=arr.length-1; i>=0; i--) {
        s += arr[i];
    }
    return s;
}
```
### 函数式编程 map-reduce：
```
function sum(arr) {
    return arr.reduce(function(prev, curr, idx, arr){
        return prev + curr;
    });
}
```
### forEach遍历:
```
function sum(arr) {
    var s = 0;
    arr.forEach(function(val, idx, arr) {
        s += val;
    }, 0);
    return s;
};
```
### eval：
```
function sum(arr) {
    return eval(arr.join("+"));
};
```
## 3.移除数组中的元素
>题目要求不改变原数组，所以可以声明一个数组a用于保存arr中不同于item的值，最后将a返回。
```
function remove(arr, item) {
     //声明一个新数组保存结果
     var a = [];
     //循环遍历
     for(var i=0; i < arr.length; i++){
         //如果arr[i]不等于item，就加入数组a
         if(arr[i] != item){
             a.push(arr[i]);
         }
     }
     return a;
 }
```
## 4.添加元素
>在数组 arr 末尾添加元素 item。不要直接修改数组 arr，结果返回新的数组。
```
/**
 * 普通的迭代拷贝
 * @param arr
 * @param item
 * @returns {Array}
 */
var append = function(arr, item) {
    var length = arr.length,
        newArr = [];
 
    for (var i = 0; i < length; i++) {
        newArr.push(arr[i]);
    }
 
    newArr.push(item);
 
    return newArr;
};
 
/**
 * 使用slice浅拷贝+push组合
 * @param arr
 * @param item
 * @returns {Blob|ArrayBuffer|Array.<T>|string}
 */
var append2 = function(arr, item) {
    var newArr = arr.slice(0);  // slice(start, end)浅拷贝数组
    newArr.push(item);
    return newArr;
};
 
/**
 * 使用concat将传入的数组或非数组值与原数组合并,组成一个新的数组并返回
 * @param arr
 * @param item
 * @returns {Array.<T>|string}
 */
var append3 = function(arr, item) {
    return arr.concat(item);
};
```
## 5.删除数组第一个元素
>删除数组 arr 第一个元素。不要直接修改数组 arr，结果返回新的数组
```
//利用slice
function curtail(arr) {
    return arr.slice(1);
}
//利用filter
function curtail(arr) {
    return arr.filter(function(v,i) {
        return i!==0;
    });
}
//利用push.apply+shift
function curtail(arr) {
    var newArr=[];
    [].push.apply(newArr, arr);
    newArr.shift();
    return newArr;
}
//利用join+split+shift    注意！！！：数据类型会变成字符型
function curtail(arr) {
    var newArr = arr.join().split(',');
    newArr.shift();
    return newArr;
}
//利用concat+shift 
function curtail(arr) {
    var newArr = arr.concat();
    newArr.shift();
    return newArr;
}
//普通的迭代拷贝
function curtail(arr) {
    var newArr=[];
    for(var i=1;i<arr.length;i++){
        newArr.push(arr[i]);
    }
    return newArr;
}
```
## 6.添加元素
>在数组 arr 的 index 处添加元素 item。不要直接修改数组 arr，结果返回新的数组。
```
//利用slice+concat
function insert(arr, item, index) {
    return arr.slice(0,index).concat(item,arr.slice(index));
}
//利用concat +splice
function insert(arr, item, index) {
    var newArr=arr.concat();
    newArr.splice(index,0,item);
    return newArr;
}
//利用slice+splice
function insert(arr, item, index) {
    var newArr=arr.slice(0);
    newArr.splice(index,0,item);
    return newArr;
}
//利用push.apply+splice
function insert(arr, item, index) {
    var newArr=[];
    [].push.apply(newArr, arr);
    newArr.splice(index,0,item);
    return newArr;
}
//普通的迭代拷贝
function insert(arr, item, index) {
    var newArr=[];
    for(var i=0;i<arr.length;i++){
        newArr.push(arr[i]);
    }
    newArr.splice(index,0,item);
    return newArr;
}
```
## 7.查找重复元素
>将传入的数组arr中的每一个元素value当作另外一个新数组b的key，然后遍历arr去访问b[value]，若b[value]不存在，则将b[value]设置为1，若b[value]存在，则将其加1。可以想象，若arr中数组没有重复的元素，则b数组中所有元素均为1；若arr数组中存在重复的元素，则在第二次访问该b[value]时，b[value]会加1，其值就为2了。最后遍历b数组，将其值大于1的元素的key存入另一个数组a中，就得到了arr中重复的元素。
```
function duplicates(arr) {
     //声明两个数组，a数组用来存放结果，b数组用来存放arr中每个元素的个数
     var a = [],b = [];
     //遍历arr，如果以arr中元素为下标的的b元素已存在，则该b元素加1，否则设置为1
     for(var i = 0; i < arr.length; i++){
         if(!b[arr[i]]){
             b[arr[i]] = 1;
             continue;
         }
         b[arr[i]]++;
     }
     //遍历b数组，将其中元素值大于1的元素下标存入a数组中
     for(var i = 0; i < b.length; i++){
         if(b[i] > 1){
             a.push(i);
         }
     }
     return a;
 }
```
## 8.时间格式化输出
```
function formatDate(t,str){
  var obj = {
    yyyy:t.getFullYear(),
    yy:(""+ t.getFullYear()).slice(-2),
    M:t.getMonth()+1,
    MM:("0"+ (t.getMonth()+1)).slice(-2),
    d:t.getDate(),
    dd:("0" + t.getDate()).slice(-2),
    H:t.getHours(),
    HH:("0" + t.getHours()).slice(-2),
    h:t.getHours() % 12,
    hh:("0"+t.getHours() % 12).slice(-2),
    m:t.getMinutes(),
    mm:("0" + t.getMinutes()).slice(-2),
    s:t.getSeconds(),
    ss:("0" + t.getSeconds()).slice(-2),
    w:['日', '一', '二', '三', '四', '五', '六'][t.getDay()]
  };
  return str.replace(/([a-z]+)/ig,function($1){return obj[$1]});
}
```
## 9.斐波那契数列
>空间复杂度：o(n)，时间复杂度：o(n)，最快 o(1)
```
var _fib = (function (n) {
   var memory = [0, 1];
   return function (n) {
       for (var i = memory.length; i <= n; i++) {
           memory[i] = memory[i - 1] + memory[i - 2];
       }
       //console.log(memory.length + ' numbers saved.');
       return memory.slice(0,n+1);
   };
})();

var fibonacci = function (n) {
   return _fib(n)[n];
}
```
>运用大数加法：
```
var _fib = (function (n) {
   var memory = ['0', '1'];

   function add(a, b) {
       var res = '',
           c = 0;
       a = a.split('');
       b = b.split('');
       while (a.length || b.length || c) {
           c += ~~a.pop() + ~~b.pop();
           res = c % 10 + res;
           c = c > 9;
       }
       return res.replace(/^0+/, '');
   }

   return function (n) {
       for (var i = memory.length; i <= n; i++) {
           memory[i] = add(memory[i - 1], memory[i - 2]);
       }
       //console.log(memory.length + ' numbers saved.');
       return memory.slice(0, n + 1);
   };
})();
```
>优化内存管理：
```
var _Fib = (function (n) {
   var memory = ['0', '1'];

   var add = function (a, b) {
       var res = '',
           c = 0;
       a = a.split('');
       b = b.split('');
       while (a.length || b.length || c) {
           c += ~~a.pop() + ~~b.pop();
           res = c % 10 + res;
           c = c > 9;
       }
       return res.replace(/^0+/, '');
   }

   return {
       get: function (n) {
           for (var i = memory.length; i <= n; i++) {
               memory[i] = add(memory[i - 1], memory[i - 2]);
           }
           //console.log(memory.length + ' numbers saved.');
           return memory.slice(0, n + 1);
       },
       clear: function () {
           //console.log('Memories reset.')
           memory = ['0', '1'];
       }
   };
})();

var fibonacci = function (n) {
   if (n < 0) {
       _Fib.clear();
   } else {
       return _Fib.get(n)[n];
   }
}


var fibonacci = function (n) {
   return _fib(n)[n];
}
```
## 10.颜色字符串转换
>将 rgb 颜色字符串转换为十六进制的形式，如 rgb(255, 255, 255) 转为 #ffffff
>1. rgb 中每个 , 后面的空格数量不固定
>2. 十六进制表达式使用六位小写字母
>3. 如果输入不符合 rgb 格式，返回原始输入
```
function rgb2hex(sRGB) {
    var regexp=/rgb\((\d+),\s*(\d+),\s*(\d+)\)/;
    var ret=sRGB.match(regexp);
    if(!ret){
        return sRGB;
    }else{
        var str='#';
        for(var i=1;i<=3;i++){
            var m=parseInt(ret[i]);
            if(m<=255&&m>=0){
                str+=(m<16?'0'+m.toString(16):m.toString(16));
            }else{
                return sRGB;
            }
        }
        return str;
    }
}
```
## 11.将字符串转换为驼峰格式
>css 中经常有类似 background-image 这种通过 - 连接的字符，通过 javascript 设置样式的时候需要将这种样式转换成 backgroundImage 驼峰格式，请完成此转换功能
>1. 以 - 为分隔符，将第二个起的非空单词首字母转为大写
>2. -webkit-border-image 转换后的结果为 webkitBorderImage
```
function cssStyle2DomStyle(sName) {
    return sName.replace(/(?!^)\-(\w)(\w+)/g, function(a, b, c){
            return b.toUpperCase() + c.toLowerCase();
        }).replace(/^\-/, '');
}
```

```
function cssStyle2DomStyle(sName) {
    var arr = sName.split('');  
    //判断第一个是不是 '-'，是的话就删除 
    if(arr.indexOf('-') == 0)
        arr.splice(0,1);
   //处理剩余的'-'
    for(var i=0; i<arr.length; i++){
        if(arr[i] == '-'){
            arr.splice(i,1);
            arr[i] = arr[i].toUpperCase();
        }
    }
    return arr.join('');
}
```
## 12.求二次方
>为数组 arr 中的每个元素求二次方。不要直接修改数组 arr，结果返回新的数组
```
function square(arr) {
    return arr.map(function(item,index,array){
        return item*item;
    })
}
```

```
function square(arr) {
   //声明一个新的数组存放结果
     var a = [];
     arr.forEach(function(e){
         //将arr中的每一个元素求平方后，加入到a数组中
         a.push(e*e);
     });
     return a;
 }
```

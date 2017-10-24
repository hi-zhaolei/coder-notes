# 排序算法

## 关于排序算法的说明

在介绍各个算法之前，我们有必要了解一下评估算法优劣的一些术语：

**稳定**：如果a原本在b前面，当a=b时，排序之后a仍然在b的前面
**不稳定**：如果a原本在b的前面，当a=b时，排序之后a可能会出现在b的后面

**内排序**：所有排序操作都在内存中完成
**外排序**：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行

**时间复杂度**：一个算法执行所耗费的时间
**空间复杂度**：运行完一个程序所需内存的大小

## 基本排序算法

基本排序算法的核心思想就是对一组数据按照一定的顺序重新排序，其中重排时一般都会用到一组嵌套的`for`循环，外循环会遍历数组的每一项元素，内循环则用于进行元素直接的比较。

### 冒泡排序（Bubble Sort）

冒泡排序是比较经典的算法之一，也是排序最慢的算法之一，因为它的实现是非常的容易的。

算法思想如下（升序排序）：

1.比较相邻的元素。如果第一个比第二个大，就交换它们两个
2.对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样最终最大数被交换到最后的位置
3.除了最后一个元素以外，针对所有的元素重复以上的步骤
4.重复步骤1~3，直到排序完成

![bubbleSort](https://user-gold-cdn.xitu.io/2017/10/21/02d839c34d647705f41bfe0fb322e154?imageView2/0/w/1280/h/960/ignore-error/1)

```js
function bubbleSort ( data ) {
    console.time('sort')
    for ( let i = data.length ; i > 0 ; i -- ){
        for( let j = 0 ; j < i - 1 ; j++){
           if( data[j] > data[j + 1] ){
               let temp = data[j];
               data[j] = data [j+1];
               data[j+1] = temp;
           }
        }
    }
    console.timeEnd('sort')
    return data;
}
bubbleSort([5,4,3,2,1])
```

### 选择排序(Selection Sort)

选择排序是一种比较简单直观的排序算法。

算法思想如下:

1.从数组的开头开始遍历，将第一个元素和其他元素分别进行比较，记录最小的元素

2.循环结束之后，将最小的元素放到数组的第一个位置上，

3.然后从数组的第二个位置开始继续执行上述步骤。当进行到数组倒数第二个位置的时候，所有的数据就完成了排序。

![selectionSort](https://user-gold-cdn.xitu.io/2017/10/21/eb7e4a78e548b29a6b96337776754128?imageView2/0/w/1280/h/960/ignore-error/1)

```js
function selectionSort (input) {
  console.time('sort')
  for(let i = 0; i <input.length; i++){
    let minIndex = i;
    let min = input[i];
    for(let j = i + 1; j <input.length; j++){
      if (min > input[j]) {
        min = input[j]
        minIndex = j
      }
    }
    const temp = input[i];
    input[i] = input[minIndex]
    input[minIndex] = temp
  }
  console.timeEnd('sort')
  return input;
}
selectionSort([5,4,3,2,1])
```

### 插入排序(Insertion Sort)

插入排序有点类似人类按字母顺序对数据进行排序，就如同你打扑克牌一样，将摸来的扑克按大小放到合适的位置一样。它的原理就是通过嵌套循环，外循环将数组元素挨个移动，而内循环则对外循环中选中的元素及它后面的元素进行比较；如果外循环中选中的元素比内循环中选中的元素小，那么数组元素会向右移动，为内循环中的这个元素腾出位置。

算法思想如下:

1.从第一个元素开始，该元素默认已经被排序
2.取出下一个元素，在已经排序的元素序列中从后向前扫描
3.如果该元素（已排序）大于新元素，将该元素移到下一位置
3.重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
4.将新元素插入到该位置
5.重复步骤2~5，直到排序完成

```js
//插入排序
function insertionSort( data ) {
    let len = data.length;
    for (let i = 1; i < len; i++) {
        let key = data[i];
        let j = i - 1;
        while ( j >= 0 && data[j] > key) {
            data[j + 1] = data[j];
            j--;
        }
        data[j + 1] = key;
    }
    return data;
}
function insertionSort(input) {
  console.time('sort')
  let output = [ -Infinity, input[0] ];
  for(let i = 1; i < input.length; i++) {
    for(let j = output.length - 1; j > -1; j--) {
      if (output[j] < input[i]) {
        output.splice(j + 1, 0, input[i])
        break;
      }
    }
  }
  console.timeEnd('sort')
  return output.slice(1)
}
insertionSort([5,4,3,2,1])
```

### 希尔排序(Shell Sort)

希尔排序又称缩小增量排序，这个算法是插入排序的改善，与插入排序不同的是，它首先会比较位置较远的元素，而非相邻的元素。
这种方案可以使离正确位置很远的元素能够快速回到合适的位置，当算法进行遍历时，所有元素的间距会不断的减小，直到数据的末尾，此时比较的就是相邻元素了。

算法思想如下:

1.先将整个待排元素序列分割成若干个子序列（由相隔某个“增量”的元素组成的）分别进行直接插入排序，
2.依次缩减增量再进行排序，待整个序列中的元素基本有序（增量足够小）时，再对全体元素进行一次直接插入排序。

因为直接插入排序在元素基本有序的情况下（接近最好情况），效率是很高的，因此希尔排序在时间效率上有较大提高。

```js
function shellSort (input) {
  console.time('sort')
  // loop(input, input.length / 2 | 0)
  let gap = input.length
  let i;
  do {
    gap = gap / 2 | 0;
    for( i = gap; i < input.length; i++) {
      if (input[i] < input[i - gap]) {
        let sortEle = input[i];
        let k = i - gap;
        while( k>=0 && input[k] > sortEle){
          input[k + gap] = input[k]
          k -= gap
        }
        input[k+gap] = sortEle
      }
    }
  } while ( gap > 1)
  console.timeEnd('sort')
  return input
}
shellSort([6,5,4,3,2,1])
```

### 归并排序(Merge Sort)

将两个的有序数列合并成一个有序数列，我们称之为"归并"，归并排序的思想就是将一系列排序好的子序列合并成一个大的完整有序的序列。

算法思想如下:

1.把长度为n的输入序列分成两个长度为n/2的子序列；
2.对这两个子序列分别采用归并排序；
2.将两个排序好的子序列合并成一个最终的排序序列

```js
function mergeSort(input) {
  function loop (data) {
    const increment = data.length / 2 | 0;
    let result = [];
    if ( increment > 1 ){
      let k = 0;
      while( k < data.length) {
        result = result.concat(loop(data.slice(k, k + increment)));
        k += increment;
      }
    } else {
      result = data;
    }
    // start insert sort
    for(let i = 1; i < result.length; i++) {
      const temp = result[i];
      let j = i - 1;
      while ( j >= 0 && result[j] > temp) {
        result[j + 1] = result[j];
        j--;
      }
      result[j + 1] = temp;
    }
    return result;
  }
  return loop(input)
}
console.time('sort')
mergeSort([6,5,4,3,2,1])
console.timeEnd('sort')
```

### 快速排序(Quick Sort)

快速排序是处理大数据最快的排序算法之一，它也是一种分而治之的算法
通过递归方式将数据依次分解为包含较小元素和较大元素的不同子序列，会不断重复这个步骤，直到所有的序列全部为有序的，最后将这些子序列一次拼接起来，就可得到排序好的数据。

算法思想如下:

1.该算法首先要从数列中选出一个元素作为基数（pivot）。
2.接着所有的数据都将围绕这个基数进行，将小于改基数的元素放在它的左边，大于或等于它的数全部放在它的右边，对左右两个小数列重复上述步骤
3.直至各区间只有1个数。

```js
function quickSort(input) {
  if (input.length < 2) return input;
  let pivot = input[0];
  let left = [];
  let right = [];
  for(let i = 0; i < input.length; i++) {
    if (input[i] < pivot) {
      left.push(input[i]);
    } else if (input[i] > pivot) {
      right.push(input[i])
    }
  }
  return quickSort(left).concat([pivot]).concat(quickSort(right));
}
console.time('sort')
quickSort([6,5,4,3,2,1])
console.timeEnd('sort')
```
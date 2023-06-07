# DSA-reminders
### Data Structures &amp; Algorithms - my own way of understanding. 
###### <p style="font-size: 1px;">(If you found this randomly, take any info with a grain of salt as it's my learning process journey (with _very high probability_ of _mistakes_) &amp; reminders for myself rather than any meaning for teaching.)</p>
---
1. **Fibonacci Sequence**
- Time Complexity: O(n) (Linear) -> 1 `for` loop.
- Math algorithm: sum up the two preceding numbers (starting from [[0, 1]](https://en.wikipedia.org/wiki/Fibonacci_sequence))
- Most of the [results](https://byjus.com/maths/fibonacci-sequence) (0 - 9).
- [Why do we start the loop from `2` instead of `0`](https://vhudyma-blog.eu/print-fibonacci-sequence-in-javascript).
```
const fibonacci = (n) => {
  const fib = [0, 1]; // commonly starts from 0 & 1
  for (let i = 2; i <= n; i++) {
  // for loop populates the `fib` array with required sequence
    fib[i] = fib[i - 1] + fib[i - 2];
  };
  return fib;
}

console.log(fibonacci(2)) // [0,1,1]
console.log(fibonacci(3)) // [0,1,1,2]
console.log(fibonacci(7)) // [0,1,1,2,3,5,8,13]
```
1.1 **Recursive Fibonacci Sequence**
- Time Complexity O(2^n) -> Quadratic worst case time because there's 2 calls for each 1 `n`th number. Past `n=40` gets too laggy (2^40).
- Iterative solution (#1) is better & faster than this recursive setup.
```
const recursiveFibonacci = (n) => {
  if (n < 2) {
    return n
  }
  return recursiveFibonacci(n - 1) + recursiveFibonacci(n - 2)
}

console.log(recursiveFibonacci(0)) // 0
console.log(recursiveFibonacci(1)) // 1
console.log(recursiveFibonacci(6)) // 8
console.log(recursiveFibonacci(7)) // 13
console.log(recursiveFibonacci(40)) // starts to get slow
```
1.2 **Recursive Fibonacci Sequence** #2
- Time Complexity O(n) (Linear) -> for each number `n` there's `n` amount of recursive calls.
- `.push` method has a Constant Time Complexity O(1).
- I *guess* space complexity suffers a bit or not hm(?) - Actually it seem like it won't hurt any space constraints.
```js
const fibRec = (n) => {
  if(n<2){
    return [0,1];
  }
  let arr = fibRec(n-1);
  arr.push(arr[arr.length-1]+arr[arr.length-2]);
  return arr;
};

fibRec(8888); // super fast
```
1.2.1 **Recursive Fibonacci Sequence** #3 Memoization Approach
- Time Complexity Linear O(n)
- Still unknown (Spanish village lol) concept, I'd have to revisit.
```
const fibRec = (n, memo = {}) => {
  if (n in memo) {
    return memo[n];
  }
  
  if (n < 2) {
    return n;
  }
  
  const result = fibRec(n - 1, memo) + fibRec(n - 2, memo);
  memo[n] = result;
  return result;
};

const fibonacciResult = fibRec(888); // Super fast again
console.log(fibonacciResult); // The Fibonacci number at index 888
```
2. **Factorial of a Number**
- Time Complexity O(n) Linear.
- Math algorithm: multiplication of all numbers between `1` and non-negative integer `n` (denoted `n!`).
- `0!` === `1`.
- `5!` === `5*4*3*2*1` = `120`.
```js
const factorial = (n) => {
  let result = 1;
  for (let i = 2; i <= n; i++) {
    result = result * i;
  }
  return result;
}

console.log(factorial(0)) // 1
console.log(factorial(1)) // 1
console.log(factorial(5)) // 120
```
3. **Prime Number**
4. **Power of Two**
5. **Linear Search**
6. **Binary Search**
7. **Insertion Sort**
8. **Quick Sort**
```js
const quickSort = arr => {
    if (arr.length <= 1) {
    // if there's 1 item inside the array -> then it's "already sorted".
        return arr;
    };
    const pivot = arr[arr.length -1];
    // const pivot = arr[Math.floor(arr.length / 2)]; // won't work with this setup
    console.log('pivot:',pivot);
    let rightArr = [];
    let leftArr = [];
    for (let i=0; i<arr.length-1; i++) {
    // for (let i=0; i<arr.length; i++) { // infinitive loop
        // if (arr[i] < arr[i+1]) { // wrong;
        if (arr[i] < pivot) {
            leftArr.push(arr[i]);
        } else {rightArr.push(arr[i])};
        console.log('looping arr:',arr); // note: logs are in reverse
    };
    console.log('FINALIZED arr:',arr); // note: logs are in reverse
    return [...quickSort(leftArr), pivot, ...quickSort(rightArr)];
};
quickSort([5,8,-55,-88,555,888]);
```
NOTE:
```js
// ...
const myArr = [5,8,-55,-88,555,888];
quickSort(myArr);
console.log('myArr:',myArr);
```
- This won't mutate the original `myArr` since I'm not modifying it directly.
- For descending order all I'd need is to swap `if (arr[i] < pivot) {` into `if (arr[i] > pivot) {`.

7.1 **Quick Sort In Place**
```js
const quickSortInPlace = (arr, left = 0, right = arr.length-1) => {
    // if (left > right) { // does nothing, exits recursive calls prematurely
    if (left < right) {
        const pivot = partition(arr, left, right);
        quickSortInPlace(arr, left, pivot-1);
        quickSortInPlace(arr, pivot+1, right);
    };
    return arr;
};

const partition = (arr, left, right) => {
    const pivot = arr[right];
    let i = left;
    for (let j = left; j < right; j++) {
        if (arr[j] < pivot) {
            swap(arr, i, j);
            i++;
        };
    };
    swap(arr, i, right);
    return i;
};

const swap = (arr, i, j) => {
    const temp = arr[i];
    arr[i] = arr [j];
    arr[j] = temp;
};

const myArr = [8, 555, -5, 888, -8];
quickSortInPlace(myArr);
console.log('myArr:',myArr);
```
- NOTE I have no idea what's going on, I'll hate to look at this later.
- NOTE#2: Such video from @codevolution doesn't exist, only a repl (https://replit.com/@Codevolution/JavaScript-Algorithms#sorting/quick-sort-in-place.js), so instead I asked ChatGPT (*as per usual, will re-modify in the future if needed.*):
  1. The `quickSortInPlace` function is the entry point for the QuickSort algorithm. It takes an array `arr`, and optional indices `left` and `right` that define the subarray to be sorted. If no indices are specified, the default values are set to the first and last indices of the array.
  2.  The function first checks `if` `left` is less (`<`) than `right`, which ensures that there is more than one element in the subarray to be sorted. If there is only one element or the indices are invalid (e.g., `left` is greater than `right`), the function returns the array as it is.
  3.    If there are multiple elements in the subarray, the function proceeds with the sorting process. It calls the `partition` function to determine the pivot index and partition the subarray.
  4.    The `partition` function takes the array `arr`, and the indices `left` and `right` defining the subarray. It chooses the rightmost element in the subarray as the pivot.
  5. The function initializes `i` as `left`, which represents the index where elements smaller than the `pivot` will be placed.
  6. It iterates over the subarray from `left` to `right - 1` using the variable `j`. For each element, it compares it with the pivot. If the element is smaller than the pivot, it swaps the element with the element at index `i` and increments `i`.
  7. After the loop, it swaps the pivot (located at index `right`) with the element at index `i`. This ensures that all elements smaller than the `pivot` are on the left side, and all elements greater than or equal to the pivot are on the right side.
  8. The function returns the index `i`, which represents the final position of the pivot.
  9. Back in the `quickSortInPlace` function, the pivot index is obtained from the `partition` function. The function recursively calls `quickSortInPlace` for the left subarray (from `left` to `pivot - 1`) and the right subarray (from `pivot + 1` to `right`).
  10. The process continues until the subarrays have only one element or are invalid. At that point, the recursive calls stop, and the function returns the sorted array.
  11. Finally, the code creates an array `myArr` and calls `quickSortInPlace` with it. The sorted array is then printed to the `console.log`.
8. **Merge Sort**

---
My most helpful JavaScript based DSA course on YouTube by @Codevolution:
#### https://www.youtube.com/playlist?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP
NOTE#1: There's mistake in his Fibonacci Sequence so I'm fixing it on my own. His mistakes are both in his video: https://www.youtube.com/watch?v=tQjd29Rmo_A&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=7 where his `for` loop has a wrong condition of `i < n` instead of `i <= n` like his correct verison at https://replit.com/@Codevolution/JavaScript-Algorithms#math/fibonacci.js -> which however he kept the *wrong comments* but the code is fixed otherwise.
  - As well as Recursive Fibonacci Sequence mistakes: https://youtu.be/wZNxLwqxu00?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=55 here he correctly shows expected returned value BUT at https://replit.com/@Codevolution/JavaScript-Algorithms#math/fibonacci-recursive.js -> he mistakenly changed argument of 3rd call from `6` to `7` but wrong expected result `8` in a comment instead should be `13`, because result `8` corresponds to Fibonacci of `6`.

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
  };
  return result;
};

console.log(factorial(0)) // 1
console.log(factorial(1)) // 1
console.log(factorial(5)) // 120
```
2.1 **Recursive Factorial of a Number**
- Time Complexity Linear O(n).
- Base case must include `n===1` check (or `n<=1`), because otherwise it will run O(n+1) => "+1" unnecessarily.
- Same time complexity as the iterative approach means the choice is only a preferation matter.
```js
function recursiveFactorial(n) {
  if (n === 0 || n===1) {
    return 1;
  };
  return n * recursiveFactorial(n - 1);
};

console.log(recursiveFactorial(0)) // 1
console.log(recursiveFactorial(1)) // 1
console.log(recursiveFactorial(5)) // 120
```
3. **Prime Number**
- Time Complexity O(n).
- Math algorithm: a whole natural number that is divisible only by `1` or itself -> in order the result to be itself.
- Also if it's divisible only by `1` like the number `1` itself is also considered not a Prime Number.
- Examples of Prime Numbers: 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37.
- Explanation of the "v2" logic below: if number divides by itself it *always* evaluates to truthy condition hence why it won't work unless I ignore the `i !== n` case or the "original" logic being: the loop not even reaching up to `n` (condition: `i<n`).
```js
const isPrime = n => {
    if (n<2) {
        // return n;
        return false;
    };
    // for (let i = 2; i <= n; i++) { // v2 to clarify the logic: going up to the number itself
    for (let i = 2; i < n; i++) { // avoids dividing it by itself in a single go
        // if (n % i === 0 && i !== n) { // v2 to clarify the logic: avoids dividing by itself
        if (n % i === 0) {
            // then it's not a Prime Number;
            return false;
        };
    };
    return true;
};
// isPrime(11);
console.log('isPrime(11):',isPrime(11)); // true
console.log('isPrime(10):',isPrime(10)); // false
console.log('isPrime(15):',isPrime(15)); // false
```
3.1 **Prime Number** for math experts solutions:
- Time Complexity O(sqrt(n)).
- Weird how there's 0 google results "O(n) vs O(sqrt(n))" -> instead all the results are against O(log n) which is faster than O(sqrt(n)). By that logic the O(sqrt(n)) is between both of them hence why it is faster/better approach than O(n) solution at #3.
- Luckily there's this [video](https://youtu.be/cbHMQxOuIUw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=392) of the series by @codevolution -> saying: if `n=100` -> code will be checking until `10` -> if `n=10000` -> code will check until `100`. => By that logic it's squared times faster (?) than O(n).
- Named as "Optimized Primality Test".
- THE LOGIC: Integers larger than the square root don't need to be checked -> whenever `n=a*b` -> one of the two factors `a` OR `b` is less than OR equal to the square root of `n`.
- Examples: `n=24`, `a=4` and `b=6` -> square root of `24` is `4.89` -> `4` is less than `4.89` === `a` is less than the square root of `n` (or of course with swapped values `a=6` and `b=4` same logic applies.).
```js
// same code just modified `for` loop's condition
// ...
for (let i = 2; i <= Math.sqrt(n); i++) {
// ...
```
4. **Power of Two**
- Time Complexity O(logn)
- The code `n=n/2` execution causes to reduce input's size by half.
- Math algorithm - given a positive integer `n`, determine if the number is a power of `2` or not. An integer is a "power of two" if tehre exists an integer `x` such that `n === 2^x`.
- Examples:
  - `1` is power of two because 2^0 is considered `true` in math.
  - `2` is power of two because 2^1 === 2.
  - `5` is *not* power of two because there's no base number that can be multiplied by itself the amount of exponent `x` times .
  - `8` is power of two because `2^3` === `8`.
  - As well as 1, 2, 4, 8, 16, 32, 64, 128, 256, 512 and more.
```js
const isPowerOfTwo = n => {
    if (n < 1) {
        return false;
    };
    while (n > 1) {
        if (n % 2 !== 0) {
            // if `n` modulus `2` is not result of `0`
            return false;
        };
        // otherwise divide `n` (since we now know it is divisible) by `2` & check again (repeat the loop)
        n = n/2;
    };
    // exiting the loop -> means `n` is now `1` and the remainder has always been `0` (always divisible by `2`)
    return true;
};
console.log(isPowerOfTwo(1)) // true
console.log(isPowerOfTwo(2)) // true
console.log(isPowerOfTwo(5)) // false
```
4.1 **Power of Two** Bitwise Solution
- Explanations:
  - In binary a number that is a "power of two", except for 1, ends with `0`:
  - 1 -> 1
  - 2 -> 10
  - 3 -> 100
  - 4 -> 1000
- That's from @codevolution on [YouTube](https://youtu.be/SZRG1bmDlx8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=420), but I still don't understand anything. Needs review.
- Unknown process to me. By the way that's all in binary representation.
```js
function isPowerOfTwoBitWise(n) {
  if (n < 1) {
    return false;
  };
  return (n & (n - 1)) === 0;
};

console.log(isPowerOfTwoBitWise(1)) // true
console.log(isPowerOfTwoBitWise(2)) // true
console.log(isPowerOfTwoBitWise(5)) // false
```
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

8.1 **Quick Sort In Place**
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

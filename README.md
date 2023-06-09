# DSA-reminders
### Data Structures &amp; Algorithms - my own way of understanding. 
###### <p style="font-size: 1px;">(If you found this randomly, take any info with a grain of salt as it's my learning process journey (with _high probability_ of _mistakes_) &amp; reminders for myself rather than any meaning for teaching.)</p>
---
1. **Fibonacci Sequence**
- Time Complexity: O(n) (Linear) -> 1 `for` loop.
- Algorithm Design Technique: **Dynamic Programming** ([more info](https://youtu.be/tCvSDnRsGnw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=160)) "which is very similar to ***Divide and Conquery*** the *only difference is you break it down into smaller but overlapping sub problems*.
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
console.log(fibonacci(8888) // remains super fast
```
1.1 **Recursive Fibonacci Sequence**
- Time Complexity O(2^n) -> Quadratic worst case time because there's 2 calls for each 1 `n`th number. Past `n=40` gets too laggy (2^40).
- Iterative solution (#1) is better & faster than this recursive setup.
```
const recursiveFibonacci = (n) => {
  if (n < 2) {
    return n;
  };
  return recursiveFibonacci(n - 1) + recursiveFibonacci(n - 2);
}

console.log(recursiveFibonacci(0)) // 0
console.log(recursiveFibonacci(1)) // 1
console.log(recursiveFibonacci(6)) // 8
console.log(recursiveFibonacci(7)) // 13
console.log(recursiveFibonacci(40)) // 102334155 // starts to get slow
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

// fibRec(8888); // super fast; as well as:
const fibArray = fibRec(8888); // super fast
console.log('fibArray[8888]:',fibArray[8888]); // Infinity // but the Algorithm remains super fast
```
1.2.1 **Recursive Fibonacci Sequence** #3 Memoization Approach
- Time Complexity Linear O(n)
- Still unknown (Spanish village lol) concept, I'd have to revisit / review.
- Notes:
- `const fibonacciResult = fibRec(8888);` runs as fast with `8888` as `888` however there's Chrome's _Range Error: Maximum call stack size exceeded._ -> Otherwise there's no infinitive loop or anything; everything is smooth.
  - The reason why #1.2 doesn't run into *Range Error* is because it returns **Array** of elements (numbers) incrementing up to the `n` number but somehow looking for the index in this code #1.2.1 is running into *Range Error*.
    - Altough I see at #1.2 I do can access the `8888`th index which is `Infinity` (hence why it seem to cause *Range Error* in here (#1.2.1) when _not_ returning an `array`, but instead an **index**; new foundings).
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

const fibonacciResult = fibRec(888); // Super fast again as well as 8888
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
- Time Complexity **O(n)** Linear hence the name.
- Algorithm Design Technique **Brute Force** ([more info](https://youtu.be/tCvSDnRsGnw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=88)).
- Search Algorithm - given an array of `n` amount of elements and a target element `target`, find the index of `target` in the array. Return `-1` if the `target` element is *not found*.
- Example: `arr = [-5, 8, 555, 10, 888]`; `target=10` -> should return 3 (index # 3).
- By me `findIndex` method under the hood.
- More info & visual explanation by freeCodeCamp [YouTube Video](https://youtu.be/8hly31xKli0?t=1070).
```js
const linearSearch = (arr, target) => {
    for (let i = 0; i < arr.length-1; i++) {
        if (arr[i] === target) {
            return i;
        };
    };
    return -1;
};
const myArr = [-5, 8, 555, 10, 888];
const target = 10;
console.log('linearSearch(myArr, target):',linearSearch(myArr, target)); // 3
console.log('linearSearch(myArr, 999):',linearSearch(myArr, 999)); // -1
```
6. **Binary Search**
- Time Complexity **O(logn)**
- In every WHILE Loop's iteration the code reduces Input's Size by HALF hence the O(logn) ([more info](https://youtu.be/75jGy1xAhhs?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=444)).
- Algorithm Design Technique: **Divide and Conquer** ([more info](https://youtu.be/tCvSDnRsGnw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=125)).
- DOWNSIDES: disadvantage in it's speed is the fact that it needs at least a same-speed performative Sorting Algorithm otherwise it'd be useless if the *Array is not sorted* already.
- Search Algorithm - given a **sorted** array of `n` amount of elements and a target element `target`, find the index of `target` in the array. Return `-1` if the `target` element is *not found*.
- Example: `arr = [-5, 8, 555, 10, 888]`; `target=10` -> should return 3 (index # 3).
  - ONLY **Sorted** array means: the `arr` argument must either be sorted or sort it first or use the alternative Linear Search Algorithm.
- Still a little bit confusing as I can't wrap up the visual part of how is the Input Size reduced by HALF.
  - ~Visual explanation~ Logical explanation by freeCodeCamp [YouTube Video](https://youtu.be/8hly31xKli0?t=2010).
    - (At timestamp above^) Search Algorithms aren't only used for numbers Data Types but it could contain any kinds of Data Types as its items.
  - Visual explanation in the same video timestamp at freeCodeCamp [YouTube Video](https://youtu.be/8hly31xKli0?t=2100).
    - At 35:44 "We repeat this process until the `target` element is **found** OR until a sublist (Python terms; otherwise JavaScript:) subArray (split Array by HALF, each piece by piece) **contains only 1 single element** -> `if` `arr[middleIndex]` matches the `target` then we know it is the `middleIndex` we're looking for OTHERWISE return `-1` if it doesn't match (`target` is *not* found in the Array).
```js
const binarySearch = (arr, target) => {
    if (arr.length === 0) {
        return -1;
    };
    
    let leftIndex = 0;
    let rightIndex = arr.length - 1;
    while (leftIndex <= rightIndex) {
        const middleIndex = Math.floor((leftIndex + rightIndex) / 2);
        
        console.log('leftIndex:',leftIndex,'& rightIndex:',rightIndex,'& sum/2 == middleIndex:',middleIndex,' & arr.length:', arr.length);
        console.log('target(item):',target,'& arr[middleIndex]:',arr[middleIndex]); // it can become a bit confusing if I try to track the items with my eyes (remove this log if it's too confusing (too many items)).
        if (target === arr[middleIndex]) {
            return middleIndex;
        };
        
        // In every WHILE Loop's iteration this code reduces Input's Size by HALF hence the O(logn)
        if (target < arr[middleIndex]) {
            rightIndex = middleIndex - 1;
        } else {
            leftIndex = middleIndex + 1;
        };
    };
    return -1;
};
// const myArr = [-5, 8, 555, 10, 888]; // Doesn't work because it's not sorted (returns -1 ALWAYS)
const mySortedArr = [-5, 8, 10, 555, 888];
const target = 10;
console.log('binarySearch(mySortedArr, target):',binarySearch(mySortedArr, target),'index.'); // 2
console.log('binarySearch(mySortedArr, 999):',binarySearch(mySortedArr, 999),'index.'); // -1
console.log('binarySearch(mySortedArr, 888):',binarySearch(mySortedArr, 8),'index.'); // 1 // very important for understanding (read explanations below)
console.log('binarySearch(mySortedArr, 999):',binarySearch(mySortedArr, -5),'index.); // 0
```
- The Pocess (still a bit confusing visually):
- The way the `.length` is shortened by a HALF is because either the left or right limits are re-assigned OR the `leftIndex` and `rightIndex` respectively TO either `middleIndex + 1` or `middleIndex - 1` respectively **ok?** 
  - So I need to cut off the wrong logic from my mind that I'm imagining as if the code is adding `+ 1` or subtracting `- 1` respectively, instead what the **most important part** is that the code _partially_ does that **but on top of** `middleIndex` (with its `Math.floor` magic), hence the **halving logic!**
    - The `middleIndex`'s `Math.floor` magic works with either or with `Math.ceil` as well as correctly! But a more common/best practice is to use `Math.floor`.
    - With that realization I noticed that when I'm using `Math.ceil` it rather increments the `middleIndex` **up to, but not** `arr.length` (hence the `arr.length-1` initial assignment to `rightIndex`), but then when I'm using `Math.floor` it decreases, but it's not decrementing like always, rather it will vary up and/or down but the original/initial `middleIndex` is the **limit**, say an example where I'm looking for `target=8` (which is at index `1` in the provided Array) in the code above the `middleIndex` goes from `2` to `0` and to `1` at which point `target=8` matches -> because:
      - Because (*into the 2nd `while` loop:*) when the `middleIndex=0` AND `rightIndex=1` AND `leftIndex=0` (hasn't changed since its initial `0`), then it checked `target=8` against item `-5` (at index `0` **because** `middleIndex=0`) and it didn't matched and -> because `target=8 < arr[middleIndex(0)]=-5` condition was **falsy** statement then -> the `else` expression had ran setting/re-assigning the `leftIndex=middleIndex(0) + 1` which made `leftIndex=1` AND `rightIndex` remained `1` => Next, `middleIndex = Math.floor((leftIndex(1) + rightIndex(1)) / 2)` === `middleIndex = Math.floor(1+1) / 2)` === `middleIndex=1` -> the `target=8 < arr[middleIndex(1)]=8` condition is **truthy** now and `return middleIndex(1)` expression had been ran which returned index `1`. 😊
        - ADDITIONALLY I also noticed that `while (leftIndex <= rightIndex)` condition came useful because my `leftIndex` and `rightIndex` were `1` both (the same index number!)! :) ✔
    - My whole point here being: ***to clarify my confusion about `middleIndex` as it doesn't always refer to the "real middle of the subArray" in a sense that I thought: 'why can't I always divide the previous `middleIndex` by `2` and then asssign it to the new `middleIndex`?'; so I need to remove from my head the wrong logical thinking that `middleIndex=middleIndex/2` is happening on every iteration because, that was a WRONG Logic on my part of trying to understand the flow!*** 👍
      - `pivotIndex` maybe could have been another good name instead of `middleIndex`.
- The `while (leftIndex <= rightIndex)` serves for when the `leftIndex === rightIndex` meaning it's the only 1 remaining item: code will still check if the `target` matches against the **Array's item** positioned at `middleIndex` (now being `0` **as well as both the `leftIndex=0` and `rightIndex=0`** if the `target` was the **very first item in the Array** OR all 3 of them being the value that equals to `arr.length-1` -> if the `target` was the **last item in the Array** (that's the **only 2 cases scenarios** when there's **only** 1 item left to be checked against)) by comparing `target === arr[middleIndex]` if the condition is **truthy** then `return middleIndex` OR if not then exit the `while` loop (then return `-1`) as there's no more items left to be checked against.

6.1 **Recursive Binary Search**
- Time Complexity **O(logn)**
  - The `if else` inside the `search` helper function _again_ reduces the Input's Size by HALF.
- The function body is taking a different approach in that it will rely on a **helper function** that'd be called recursively ([more info](https://youtu.be/EFXWgZJZqL8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=200)).
- The process is a bit confusing to me in the fact that `search` helper function takes in 4 Arguments -> but it clarifies once I wrap up my thoughts in that there's already 2 Arguments in the non-recursive way, so the extra 2 Arguments are the `leftIndex` and `rightIndex` because this `search` helper function holds the whole logic.
  - Now the `search` helper function must call itself inside the `if else` conditions.
- Search Algorithm that, again, **must only** be searching in an already **sorted** array, else sort it myself first.
```js
const recursiveBinarySearch = (arr, target) => {
  return search(arr, target, 0, arr.length - 1);
};

const search = (arr, target, leftIndex, rightIndex) => {
  if (leftIndex > rightIndex) {
    return -1;
  };

  let middleIndex = Math.floor((leftIndex + rightIndex) / 2)
  if (target === arr[middleIndex]) {
    return middleIndex;
  };

  // This condition again reduces the Input's size by HALF.
  if (target < arr[middleIndex]) {
    return search(arr, target, leftIndex, middleIndex - 1);
  } else {
    return search(arr, target, middleIndex + 1, rightIndex);
  };
};

console.log(recursiveBinarySearch([-5, 8, 10, 555, 888], 10)); // 2
console.log(recursiveBinarySearch([-5, 8, 10, 555, 888], 666)); // -1
console.log(recursiveBinarySearch([-5, 8, 10, 555, 888], 888)); // 4
```
7. **Bubble Sort**
- Time Complexity **O(n^2) Quadratic**
  - Bubble Sort is a [poor](https://youtu.be/gqMjdM8FsrE?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=40) Sorting Algorithm & it's only used for introduction into understanding the fundamentals of Sorting Algorithms. Should almost never be used unless asked about it in an interview.
- Sorting Algorithm: Given an array of integers, sort the array.
  - Example: `arr = [5,8,-55,-88,555,888]` -> `bubbleSort(arr)` => should return `[-88, -55, 5, 8 , 555, 888]`.
- The logic as per @Codevolution [YouTube Video](https://youtu.be/gqMjdM8FsrE?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=80):
  - Compare adjacent elements (neighbouring items) in the Array && swap the positions `if` they are **not** in the intended order.
  - Repeat the instruction as you step through each element in the Array.
  - Once you step through the whole Array with no Swaps -> it means: ***the Array is sorted***.
- The `for` loop's condition `i < arr.length -1` stops at `-1` because the code `if (arr[i] > arr[i+1])` comparison check is being made against `arr[i+1]` ([more info](https://youtu.be/xdCgW2a3r_Q?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=117)) -> there's no need ot comapre the last sorted element against an element outside the array (like he says).
- The logic behind `swapped` in the same video timestamp at [YouTube Video](https://youtu.be/xdCgW2a3r_Q?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=183).
- The whole code summary in the same video [timestamp at 4:00](https://youtu.be/xdCgW2a3r_Q?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=240).
  - "*We begin by going through the Array **at least once**, we **check** (`if (arr[i] > arr[i + 1]`) if any adjacent elements are out of order, if they're not -> we **exit** the `do while` loop since the Array **is already sorted**. If we did **swap** elements -> we go through the Array **again** to make sure there's no more **swapping** required -> if we encounter a **swap** then repeat the process -> if we don't encounter a **swapp** then the Array has been **already sorted***.".
  - Aha-moments by me: use cases in real life for this piece of code (the `if` condition) can be used as `isArraySorted` function checker OR helper function for Binary Search Algorithm which **only** works **`if`** the Array **is already sorted**:
    - But *of course* don't use this poor Bubble Sort, but rather some of the better Sorting Algorithms below -> here I'm only talking about piece of the code that can be used as **is Array already sorted** helper function, but omit the *swapping* part of the code.
```       js
const bubbleSort = (arr) => {
  let swapped;
  do {
    swapped = false;
    for (let i = 0; i < arr.length - 1; i++) {
      if (arr[i] > arr[i + 1]) { // swap ">" with "<" => and there's descending ordering
        let temp = arr[i];
        arr[i] = arr[i + 1];
        arr[i + 1] = temp;
        swapped = true;
      }
    }
  } while (swapped);
};
const arr = [5, 8, -55, -88, 555, 888]; // will get mutated if I don't make copy a
// inside `bubbleSort` which might need a Deep Copy (iterating over each elements).
bubbleSort(arr);
console.log('FINALIZED arr:', arr); // [-88, -55, 5, 8, 555, 888]
```
8. **Insertion Sort**
- Time Complexity
- Visual Explanation of Insertion Sort at the [YouTube Video](https://youtu.be/Wu_mDUIsTVE?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=100)'s timestamp by @Codevolution.
```                   js

```
9. **Quick Sort**
- - Algorithm Design Technique: **Divide and Conquer** ([more info](https://youtu.be/tCvSDnRsGnw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=125)).
- Sorting Algorithm.
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

9.1 **Quick Sort In Place**
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
10. **Merge Sort**
- Algorithm Design Technique: **Divide and Conquer** ([more info](https://youtu.be/tCvSDnRsGnw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=125)).
- Sorting Algorithm
```js
```

---
My most helpful JavaScript based DSA course on YouTube by @Codevolution:
#### https://www.youtube.com/playlist?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP
NOTE#1: There's mistake in his Fibonacci Sequence so I'm fixing it on my own. His mistakes are both in his video: https://www.youtube.com/watch?v=tQjd29Rmo_A&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=7 where his `for` loop has a wrong condition of `i < n` instead of `i <= n` like his correct verison at https://replit.com/@Codevolution/JavaScript-Algorithms#math/fibonacci.js -> which however he kept the *wrong comments* but the code is fixed otherwise.
  - As well as Recursive Fibonacci Sequence mistakes: https://youtu.be/wZNxLwqxu00?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=55 here he correctly shows expected returned value BUT at https://replit.com/@Codevolution/JavaScript-Algorithms#math/fibonacci-recursive.js -> he mistakenly changed argument of 3rd call from `6` to `7` but wrong expected result `8` in a comment instead should be `13`, because result `8` corresponds to Fibonacci of `6`.

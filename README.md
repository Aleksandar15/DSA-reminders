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
  - I'm also wondering if I can use this very same Bubble Sort part of the code that **checks if the Array is already sorted** as a helper function inside Quick Sort Algorithm -> as to avoid the Worst Case Time Complexity O(n^2) Quadratic inside the Quick Sort Algorithm?
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
const arr = [5, 8, -55, -88, 555, 888]; // will get mutated if I don't make a copy
// inside `bubbleSort` which might need a Deep Copy (iterating over each elements).
bubbleSort(arr);
console.log('FINALIZED arr:', arr); // [-88, -55, 5, 8, 555, 888]
```
8. **Insertion Sort**
- Time Complexity **O(n^2) Quadratic**
  - A `while` loop nested inside `for` loop.
  - [6:15](https://youtu.be/OxUF23k7IcM?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=375)]: "As the number of elements in the Array increases -> the Number of Comparisons (# Amount of Comparisons) inceases by square of that number.".
- Visual Explanation of Insertion Sort at the [YouTube Video](https://youtu.be/Wu_mDUIsTVE?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=100)'s timestamp by @Codevolution.
  - 2:10 starting at index `1` we have the element at the index `1` which is called ***Number TO Insert*** or ***NTI*** for short in his graph. -> The ***Sorted Element*** is represented as ***SE*** for short.
  - And in his next video at [this timestamp](https://youtu.be/OxUF23k7IcM?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=48) he repeats: "In Assertion Sort the first element is assumed to be sorted & we need to **traverse** the ***unsorted part of the Array**".
- I honestly don't understand Insertion Sort very well, I'd need to rewatch the video OR review the code myself & write my own notes of code breakdown.
- Sorting Algorithm - Given an array of integers, sort the array.
```                   js
const insertionSort = (arr) => {
  for (let i = 1; i < arr.length; i++) { // i = 1 -> because 1st element is assumed to be sorted
    let numberToInsert = arr[i]; // assigned with LET so that they can be re-assigned
    let j = i - 1; // assigned with LET so that they can be re-assigned
    while (j >= 0 && arr[j] > numberToInsert) {
      arr[j + 1] = arr[j];
      j = j - 1;
    };
    arr[j + 1] = numberToInsert;
  };
};
const arr = [5, 8, -55, -88, 555, 888]; // will get mutated if I don't make a copy 
// inside `insertionSort` which might need a Deep Copy (iterating over each elements).
insertionSort(arr);
console.log('FINALIZED arr:', arr);
```
9. **Quick Sort**
- Time Complexity Worst Case **O(n^2) Quadratic** -> "_this happens when they Array is already sorted_" ([explanation](https://youtu.be/lWLTHsQnHDI?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=222)) - he says; so I'm wondering if I can use the Bubble's Sort part of the code that checks if the Array is already sorted -> as to avoid the Worst Case Time Complexity in this Quick Sort Algorithm?
  - 4:00 "You end up comparing with **every other element** and that's why it is Quadratic Time Complexity".
- Time Complexity Average Case **O(nlogn) Linearithmic**
  - 4:04 "Quick Sort Algorithm is a popular Algorithm because of it's Averaga Case Complexity Linearithmic O(nlogn)".
  - 4:20 "We recursively divide the Array into smaller Arrays (subArrays) which is **O(logn)**" -> Highlights this part of the code `[...quickSort(leftArr), pivot, ...quickSort(rightArr)]`.
  - 4:23 "We also have a `for` loop which is **O(n)**."
  - 4:28 "Combine the two and we have **O(nlogn)**".
  - 4:32 "The way to derive/to calculate this Time Complexity is Complex & out of scope for this video series".
  - 5:15 "If you don't have any Space Complexity Constrains (Auxiliary Space Constrains) you can go with this Regular Quick Sort instead of the In-Place Quick Sort OR Merge Sort Algorithm (below)".
- Algorithm Design Technique: **Divide and Conquer** ([more info](https://youtu.be/tCvSDnRsGnw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=125)).
  - Hence the piece of code `if (arr.length <= 1)` Base Case for the recursive calls suggests Array is Already Sorted -> if there's only 1 element remaining inside the Array (as is the similar case with Merge Sort Algorithm's [explanations](https://youtu.be/qInXNtKaf4Q?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=66)).
  - Scroll down to 1:08 where the explanation for Quick Sort is written ***as to why there has to be 1 element remaining***.
    - I see a similar logic like the Bubble Sort's notes I wrote about a possible `isArraySorted` function (or `isArrayAlreadySorted` helper function).
- Sorting Algorithm - Given an array of integers, sort the array.
- Quick Sort idea by @codevolution [YouTube Video](https://youtu.be/ceqwscS_muA?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=42):
 - 0:42 Identify the `pivot` element in the Array (choose one of the options as pivot):
   - Pick first element as `pivot`.
   - Pick last element as `pivot` (Our Approach).
   - Pick a random element as `pivot`.
   - Pick median as `pivot`.
 - 1:08 What role does the `pivot` plays? You traverse the Array from the **first** element to the **last** but **ONE** element remaining (1 element remaining), and you (`.push`) put everything that's smaller than the `pivot` element into the `leftArray` AND everything that's greater than the `pivot` element into the `rightArray`. -> Repeat the process until for the individual `leftArray` AND `rightArray` until there's an Array of `length` 1 length.
 - 1:37 ***An Array of `length` 1 is sorted by definition.***
 - 1:44 When that **Base Condition** is reached (`if (arr.length <= 1)`) -> concatenate the `leftArray` + `pivot` + `rightArray` respectively in the **same order.**
- EXTRAS:
- ChatGPT says: "To mitigate the chance of worst-case behavior, various optimizations and techniques can be applied, such as choosing a random pivot, using median-of-three pivot selection, or implementing hybrid sorting algorithms that switch to a different sorting method for smaller subarrays."
  - Which Google results I've found:
  - [Advanced Quick Sort (Hybrid Algorithm)](https://www.geeksforgeeks.org/advanced-quick-sort-hybrid-algorithm) by GeeksForGeeks.
  - [QuickSort using Random Pivoting](https://www.geeksforgeeks.org/quicksort-using-random-pivoting) by GeeksForGeeks.
- GeeksForGeeks also call the Space Complexity as Auxiliary Space Complexity (sometimes even omitting the "Complexity" word).
```js
const quickSort = arr => {
    if (arr.length <= 1) { // Base Case for the recursive calls
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
- Time Complexity Worst Case **O(n^2)** Quadratic
- Time Complexity Average Case **O(nlogn)** Linearithmic
- Both the ***Regular* Quick Sort** and **In-Place Quick Sort** Algorithms have the same average-case time complexity of O(n log(n)), but the in-place quick sort has a better space complexity of O(log(n)) compared to the regular quick sort's potential **O(n) Space Complexity**.
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
- Time Complexity **O(nlogn)** Logarithmic Sorting is the Best Sorting Time Complexity you can build code-wise ([6:58](https://youtu.be/wXZyuJqNk9U?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=420)). -> BUT ***WARNING:*** There's a **mistake** on the Video Creator part (@codevolution) using `.shift` (O(n)) inside a loop becomes O(n^2) Quadratic Time Complexity && is confirmed the comments in the Video Hyperlinked below:
  - His breakdown at [6:20](https://youtu.be/wXZyuJqNk9U?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=380):
  - 6:20 Recursively divide the Input size into halves(/divide the problem into halves) this is Time Complexity O(logn).
  - 6:30 Second part we merge the Arrays and this contains a `while` loop -> if there's a loop the Time Complexity is Linear O(n) -> **HERE's where the author is wrong; missing to see the expensive `.shift` inside the `while` loop**.
  - 6:40 Our solution is a combination of the two: O(nlogn) Linearithmic Time Complexity also called Loglinear (names I found in Google; not by Creator).
- Algorithm Design Technique: **Divide and Conquer** ([more info](https://youtu.be/tCvSDnRsGnw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=125)).
- Sorting Algorithm - Given an array of integers, sort the array.
- Merge Sort idea by @codevolution [YouTube Video](https://youtu.be/qInXNtKaf4Q?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=42):
  - 0:42 First divide the Array into **subArray** each containing **only 1 element** -> ***An Array with one element remaining considered sorted by definition.***
  - 0:53 Step 2: Repeteadly merge the **subArrays** to produce new **Sorted subArrays** until there's only **1 subArray remaining** -> which will be the **Sorted Array** by the same definition/same logic.
- Explaining The Process:
- Visual explanation by [@codevolution YouTube Video at timestamp](https://youtu.be/qInXNtKaf4Q?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=66).
  - 1:23 There's 2 Part Process in Merge Sort:
  - 1:25 **1st you divide the Array until you are left with subArrays that contain *only* 1 element** remaining -> the logic for that is to split the Array in the Middle until you have an Array of length `1` (keep splitting the Array into middle and then use `Math.floor` for `middleIndex` (the same case is with the Quick Sort Algorithm).
  - 2:36 2nd Step: We **merge** the individual subArrays into new subArrays while **ensuring** the **elements are sorted**.
  - 2:50 Here's how it works: We take the 2 Arrays and a **temporary empty Array** to hold the Elements as they are Sorted (I guess to hold the sorted Arrays(?)).
- I've googled a bunch to confirm if the group thinking inside the comment section was right & results are confusing how many GOOGLE TOP RESULTS shows using `.shift` method AND there's scarcity amount of Merge Sort JavaScript results ***even when I'm specific in my searches*** using advanced google searching method like wrapping my search terms in a strings 
- **UPDATE:**
- - -> **[Using any of the Array methods with Linear Time Complexity O(n) inside of a Loop becomes a Quadratic Time Complexity O(n^2)](https://youtu.be/txjmvEPlAtU?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=396)** (_Video by @codevolution about Array Data Structure_).
  - Which he himself confirms that his Merge Sort indeed was a mistake using `.shift` method inside of a `while` loop! (*he confirms indirectly*.)
- *Update ends here, the rest continue from before:*
- -> I'm wondering how people even got a job at FAANG if Google's TOP RESULTs "`merge sort javascript`" have mistakes:
  - https://www.doabledanny.com/merge-sort-javascript uses `.shift`.
    - Additionally saying the `merge` algorithm runs in O(n+m) (or O(n + m)).
  - https://stackabuse.com/merge-sort-in-javascript uses `.shift.
  - https://www.tutorialspoint.com/how-to-implement-merge-sort-in-javascript uses `.shift`.
  - A good one below:
  - https://medium.com/@renxburnett/merge-sort-in-javascript-849cf7527450 uses 3 `while` loops like my # 10.0.1 solution; but doesn't mention nothing against `.shift`.
    - Also the author calls this one of the hardest coding challenge he came across.
  - https://medium.com/analytics-vidhya/implement-merge-sort-algorithm-in-javascript-7402b7271887 uses 3 `while` loops like my # 10.0.1.
  - https://javascript.plainenglish.io/javascript-merge-sort-3205891ac060 uses my # 10.0.2
- The worst mystery is that google search results for `"is shift method inside a loop expensive"` AND the more broad `"is o(n) inside a loop expensive"` shows no _focused_ results; only side-topics.
- Not even google results for `"javascript merge sort with pointers"` doesn't show any *focused* results (I'm frustrated, but I'll ignore it for now without deep diving into this topic (_I can't even if I wanted to_, there's 0 focused google search result & ChatGPT agreeing with everything I say)).
- **MISTAKEN CODE BELOW BY AUTHOR @codevolution HAS MISTAKES AS IT IS O(n^2) Quadratic and NOT O(nlogn) Linearithmic!** (Scroll below for my own solution.)
```js
const mergeSort = (arr) => {
  if (arr.length <= 1) {
    return arr;
  };
  const middleIndex = Math.floor(arr.length / 2);
  const leftArr = arr.slice(0, middleIndex);
  const rightArr = arr.slice(middleIndex);
  return merge(mergesort(leftArr), mergesort(rightArr));
};

const merge = (leftArr, rightArr) => {
  const sortedArr = [];
  while (leftArr.length && rightArr.length) {
    if (leftArr[0] <= rightArr[0]) {
      sortedArr.push(leftArr.shift()); // .shift Becomes O(n^2) Quadratic Time Complexity mistake by Video Creator @codevolution
    } else {
      sortedArr.push(rightArr.shift()); // .shift Becomes O(n^2) Quadratic Time Complexity mistake by Video Creator @codevolution
    };
  };
  const resultArr = [...sortedArr, ...leftArr, ...rightArr];
  return resultArr;
};
const arr = [5, 8, -55, -88, 555, 888]; // will get mutated if I don't make a copy 
// inside `mergeSort` which might need a Deep Copy (iterating over each elements).
console.log('FINALIZED arr:', arr);
```
10.0.1 **UPDATED CODE TO FIX HIS MISTAKES:** (but it's quite redundant `while` loops I see)
```js
const mergeSort = (arr) => {
  // Base case: if the array is empty or contains only one element, it is already sorted
  if (arr.length <= 1) { // I prefer over "< 2"
    return arr;
  }

  const middleIndex = Math.floor(arr.length / 2);
  // Split the Array into 2 halves
  const leftArr = arr.slice(0, middleIndex);
  const rightArr = arr.slice(middleIndex);

  // Merge the sorted halved Arrays (which are recursively sorted themselves; hence the nested recursive calls)
  return merge(mergeSort(leftArr), mergeSort(rightArr));
};

const merge = (leftArr, rightArr) => {
  const sortedArr = [];
  let leftPointer = 0;
  let rightPointer = 0;

  // Compare elements from the left and right arrays and merge them in sorted order
  while (leftPointer < leftArr.length && rightPointer < rightArr.length) {
    // console.count('1st while runs');
    if (leftArr[leftPointer] <= rightArr[rightPointer]) {
      sortedArr.push(leftArr[leftPointer]);
      leftPointer++;
    } else {
      sortedArr.push(rightArr[rightPointer]);
      rightPointer++;
    }
  }

  // Add the remaining elements from the left or/and right array (these run but not always)
  while (leftPointer < leftArr.length) {
    // console.count('2nd while runs');
    sortedArr.push(leftArr[leftPointer]);
    leftPointer++;
  }

  while (rightPointer < rightArr.length) {
    // console.count('3rd while runs');
    sortedArr.push(rightArr[rightPointer]);
    rightPointer++;
  }

  return sortedArr;
};

const arr = [5, 8, -55, -88, 555, 888]; // will get mutated if I don't make a copy 
// inside `mergeSort` which might need a Deep Copy (iterating over each elements).
// mergeSort(arr);
// console.log('FINALIZED arr:', arr);
// // To avoid modifying the original Array OF INTEGERS let's pass a SHALLOW COPY
// // (otherwise more complex Arrays would need a DEEP COPY or using Immer.js):
const sortedArr = mergeSort([...arr]); // Create a copy to avoid mutation
console.log('ORIGINAL arr:', arr);
console.log('Sorted Array:', sortedArr);
```
10.0.2 To shorthen the redundant `while` loops (creating overheads) in the # 10.0.1 code but logic remains the same:
- NOTE I have to re-google to confirm if using `.concat` over Spread Syntax is more efficient/faster in a large Arrays.
```js
const mergeSort = (arr) => {
  // Base case: if the array is empty or contains only one element, it is already sorted
  if (arr.length <= 1) { // I prefer over "< 2"
    return arr;
  }

  const middleIndex = Math.floor(arr.length / 2);
  const leftArr = arr.slice(0, middleIndex);
  const rightArr = arr.slice(middleIndex);

  return merge(mergeSort(leftArr), mergeSort(rightArr));
};

const merge = (leftArr, rightArr) => {
  const sortedArr = [];
  let [leftPointer, rightPointer] = [0, 0]; // this one liner is equivalent to:
  // let leftPointer = 0; // and "leftArr[leftPoitner++]" increments it
  // let rightPointer = 0; // and "rightArr[rightPointer++]" increments it

  // Compare elements from the left and right arrays and merge them in sorted order
  while (leftPointer < leftArr.length && rightPointer < rightArr.length) {
    // One Liner:
    sortedArr.push(leftArr[leftPointer] <= rightArr[rightPointer] ? leftArr[leftPointer++] : rightArr[rightPointer++]);
    // Alternative To:
    // if (leftArr[leftPointer] <= rightArr[rightPointer]) {
      // sortedArr.push(leftArr[leftPointer++]); // works
      // // // but below:
      // // // leftPointer++; // DOESN'T work here (fills the Array with UNDEFINED elementsrandomly):
      // // sortedArr.push(leftArr[leftPointer]);
      // // leftPointer++; // works if it's here
    // } else {
      // sortedArr.push(rightArr[rightPointer++]); // works
      // // // but below:
      // // // rightPointer++; // DOESN'T work here (fills the Array with UNDEFINED elementsrandomly):
      // // sortedArr.push(rightArr[rightPointer]);
      // // rightPointer++; // works if it's here
    // }
  }

  // return sortedArr.concat(leftArr.slice(leftPointer)).concat(rightArr.slice(rightPointer)); // works
  return [...sortedArr, ...leftArr.slice(leftPointer), ...rightArr.slice(rightPointer)]; // works as fine
};

const arr = [5, 8, -55, -88, 555, 888];
const sortedArr = mergeSort([...arr]); // Create a SHALLOW COPY to avoid mutation of integers, but complex Arrays require DEEP COPY
console.log('ORIGINAL arr:', arr);
console.log('Sorted Array:', sortedArr);
```
- NOTES Dangerous Code Spontaniously Found
- I noticed in my testing it will crash my YouTube tab & all it's nearby tabs if I modify **only** the line of code above as:
- (I feel like I hacked myself lol && Everytime I run code above with modified line below it either refreshes random tabs or kills random tabs.)
```
// ...
sortedArr.push(leftArr[leftPointer] <= rightArr[rightPointer] ? leftArr[0] : rightArr[rightPointer++]);
// ...
```
11. Cartesian Product
- Time Complexity **O(n*m)** (*@codevolution calls it O(mn)*).
  - Rare Time Complexity.
  - `arr1` is of `length` `n` AND `arr2` is of `length` `m`.
  - However if the **`length` of the both Arrays** matches then it's **O(n^2) Quadratic Time Complexity** ([more info](https://youtu.be/XPpnW-WsDmQ?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=213)).
  - So which one is faster?
  - https://stackoverflow.com/questions/35855978/is-omn-in-on2:
  - Seems like stackoverflow also calls it **O(mn) Matrix Time Complexity** (O(m*n)).
  - "It's a matter of if `m` is greater than `n`: if `m > n`, then **O(n^2) is faster**; else if `m < n`, then **O(mn) is faster.".**
  - (By me: I guess this is some Matrix Algorithm, but seems like a similar rules apply here.)
- Helpful [YouTube video](https://www.youtube.com/watch?v=C2HuBFYgyM8&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=29&pp=iAQB) by @codevolution.
- Misc Problems of Time Complexity (Miscellaneous Problems of Time Complexity).
- Misc Problem - Given two finite non-empty sets, find their Cartesian Product.
- Examples:
- `const A = ['1A', '2A'];`
- `const B = ['1B', '2B', '3B'];`
- RESULTS: AxB `= [['1A', '1B',], ['1A', 2B'], ['1A', '3B'], ['2A', '1B'], ['2A', '2B'], ['2A', '3B']];`
- **SOLUTION:**
```js
const cartesianProduct = (arr1, arr2) => {
  const result = [];
  for (let i = 0; i < arr1.length; i++) {
    for (let j = 0; j < arr2.length; j++) {
      result.push([arr1[i], arr2[j]]);
    };
  };
  return result;
};

const arr1 = [1, 2];
const arr2 = [3, 4, 5];
console.log(cartesianProduct(arr1, arr2)); // [[1, 3], [1, 4], [1, 5], [2, 3], [2, 4], [2, 5]]
```
12. Climbing Staircase
- Time Complexity **O(n)** Linear (1 single `for` loop).
- Helpful [YouTube video](https://www.youtube.com/watch?v=jrY7eONLHZs&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=31&pp=iAQB) by @codevolution.
- Misc Problems of Time Complexity (Miscellaneous Problems of Time Complexity).
- Misc Problem - Given a staircase of `n` steps, count the number of distinct ways to climb to the top. -> You can either climb 1 step or 2 steps at a time.
- Examples:
- `n=1`, `climbingStaircase(1)` = `1` -> there's **only 1** way to climb to the top: (1)
- `n=2`, `climbingStaircase(2)` = `2` -> there are **2** ways to climb: (1,1) "1 at a time" OR (2) "2 steps at once"
- `n=3`, `climbingStaircase(3)` = `3` -> there are **3** ways to climb: (1,1,1) (1 at a time) OR (1,2) OR (2,1)
- `n=4`, `climbingStaircase(4)` = `5` -> there are **5** ways to climb to the top: (1,1,1,1) OR (1,1,2) OR (1,2,1) OR (2,1,1) OR (2,2)
- Climbing Staircase idea breakdown:
- At any given time you can climb either 1 step or 2 steps at a time.
- If you have to climb to step `n`, you can **only** climb from step `n-1` or `n-2` there's no other way.
- Calculate the way you cna climb to `n-1` and `n-2` steps and **add the two**.
- `climbingStaircase(n)` = `climbingStaircase(n-1) + climbingStaircase(n-2)` -> Same pattern as Fibonacci sequence.
- The numbers of ways to climb `n` is equal to the number of ways to climb `n-1` + the number of ways to climb `n-2`.
```js
const climbingStaircase = (n) => {
  const numberOfWays = [1, 2];

  // Start at i=2 -> since Arrays are 0 indexed (and we already know about 0 and 1)
  for (let i = 2; i <= n; i++) {
    numberOfWays[i] = numberOfWays[i - 1] + numberOfWays[i - 2];
    console.log("LOOP numberOfWays:", numberOfWays);
  };

  console.log("FINAL numberOfWays:", numberOfWays);

  return numberOfWays[n - 1];
};

console.log(1+':', climbingStaircase(1)); // 1
console.log(2+':', climbingStaircase(2)); // 2
console.log(3+':', climbingStaircase(3)); // 3
console.log(4+':', climbingStaircase(4)); // 5
console.log(5+':', climbingStaircase(5)); // 8
```
13. Tower of Hanoi
- Time Complexity **O(2^n) Exponential Time Complexity *is pretty bad I see*.**
- Time Complexity is understood by a pattern ([5:25](https://youtu.be/bLHxrvDvL_8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=333)) -> but for deriving the Time Complexity you can Google **Master Theorem** helps to calculate Time Complexity when Recursion is involved.
  - 6:10 the trend is **2^n-1** -> which the **Worst Case Time Complexity is O(2^n).** 
  - Since it has a Time Complexity O(2^n) it gets too slow once Disks amount is `n=15` or above.
    - It's funny though, and I like the fact how `console.log`s keep printing -> a pretty gamey experience to be honest.
- Helpful [YouTube video](https://www.youtube.com/watch?v=_dt773ImwFw&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=33&pp=iAQB) by @codevolution.
- Misc Problems of Time Complexity (Miscellaneous Problems of Time Complexity).
- Misc Problem - Objective of the Puzzle is to move the entire stack to the **last rod**, obeying the following rules:
  - **Only** one disk may be moved at a time.
  - Each move consists of taking the **upper disk** from one of the **stacks** and **placing** it on **top** of **another stack** or an **empty rod.**
  - **No** disk may be placed on **top** of a **disk** that is **smaller**.
- Visual Explanation at a [timestamp](https://youtu.be/_dt773ImwFw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=50).
- The [steps](https://youtu.be/_dt773ImwFw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=100):
  - Step 1: Shift `n-1` disks from `A` to `B`, using `C` (when required). -> *By me: why using `C` I don't understand?*
  - Step 2: Shift **last** disk from `A` directly to `C`.
  - Step 3: Shift `n-1` disks from `B` to `C`, using `A` (when required). -> *By me: why using `A` I don't understand?*
  - 2:00 is Visual Explanations using 3 Disks instead of 2 Disks. -> Same logic applies here.
  - 3:00 Nope I personally don't understand anything. Needs review / revisit on my part.
- The solution code explanations:
  - [1:05](https://youtu.be/bLHxrvDvL_8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=63) Solution code is using recursive approach.
  - 1:10 The **Base Case** is the second step "Shift **last** disk from `A` to `C`". -> Code-wise: `if (n===1)`.
  - 2:00 The **Recursiive Calls** is handled by step one "Shift `n-1` disks from `A` to `B`, using `C` (when required)".
    - The code is: `towerOfHanoi(n-1, fromRod, usingRod, toRod)`; meaning when called as:
    - `towerOfHanoi(3, 'A', 'C', 'B')` -> Move `n-1` Disk from `'A'` to `'B'` using `'C'`.
  - 3:00 Finally step 3 -> "Shift `n-1` disks from `B` to `C`, using `A` (when required)"
    - The code is: `towerOfHanoi(n-1, usingRod, toRod, fromRod)`; meaning when called as:
    - `towerOfHanoi(3, 'A', 'C', 'B')` -> Move `n-1` Disk from `'B'` to `'C'` using `'A'`.
- `Return` value doesn't matter here rather it's the `console.log`s that matters in this gamey code of what seems to be a Puzzle Game code or a Mathematical Puzzle Game code to be precise.
```js
const towerOfHanoi = (n, fromRod, toRod, usingRod) => {
    // Base case is the Step 2
    if (n === 1) {
        // Step 2
        console.log(`Move disk 1 from ${fromRod} to ${toRod}`);
        return;
    };
    // Step 1
    towerOfHanoi(n-1, fromRod, usingRod, toRod);
    console.log(`Move disk ${n} from ${fromRod} to ${toRod}`);
    // Step 3
    towerOfHanoi(n-1, usingRod, toRod, fromRod);
};

towerOfHanoi(2, 'A', 'C', 'B','F'); // Return value doesn't matter it's the console.log that matters
// Since it is of Time Complexity O(2^n) it gets too slow once Disks amount is 15 or more.
```
---
# DATA STRUCTURES
#### There's [video series](https://www.youtube.com/watch?v=poGEVboh9Rw&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=36&pp=iAQB) by @codevolution about DS continuation from the Algorithms of the full DSA playlist.
- Learning about Data Structure helps to gain a more knowledge about things we are already using such as ([3:33](https://youtu.be/poGEVboh9Rw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=213)):
  1. Document Object Model or DOM uses **Tree Data Structure**.
  2. Browser's back and forward button uses **Stack Data Structure.**
  3. Operating Systems or OS Job Scheduling uses **Queue Data Structure.**
- Inbuilt or **Built-In Data Structures** in JavaScript are:
  1. Arrays Data Structure
  2. Objects Data Structure
  3. Sets Data Structure
  4. Maps Data Structure
- Custom Data Structures in JavaScript _are created using Built-in Data Structure_ and are:
  1. Stacks Data Structure
  2. Queues Data Structure
  3. Circucal Queues Data Structure
  4. Linked Lists Data Structure
  5. Hash Tables Data Structure
  6. Trees Data Structure
  7. Graphs Data Structure
1. Array Data Structure
- An Array is a  Data Structure that can hold a collection of values with a mix of different data types such as Strings, Booleans, Numbers, or even Objects themselves (*well the pointers of the Object in the actuallity*).
- Accessing element using square brackets such as syntax `arr[0]`. Constant Time Complexity O(1)
- [`.push`](https://youtu.be/txjmvEPlAtU?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=180) Method **adds** an element to the **back** of the Array. Constant Time Complexity O(1)
- `.pop` method **removes** an element from the **back** of the Array. Constant Time Complexity O(1)
- `.unshift` Method **adds** an Element to the **front** of the Array. Linear Time Complexity O(n)
- `.shift` method **removes** an elemenet from the **front** of the Array. Linear Time Complexity O(n)
- Other Array Methods:
- `.map` Linear Time Complexity O(n)
- `.filter` Linear Time Complexity O(n)
- `.forEach` Linear Time Complexity O(n)
- `.reduce` Linear Time Complexity O(n)
- `.concat` Linear Time Complexity O(n)
- `.slice` Linear Time Complexity O(n)
- `.splice` Linear Time Complexity O(n)
- -> **[Using any of the methods above of Linear Time Complexity O(n) inside of a Loop becomes a Quadratic Time Complexity O(n^2)](https://youtu.be/txjmvEPlAtU?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=396)**
  - Which he himself confirms that his Merge Sort indeed was a mistake using `.shift` method inside of a `while` loop! (*he confirms indirectly*.)
    - He @codevolution says "the interviewer won't be happy with that" -> yet he gives me such a mistaken/misleading solution in the Merge Sort Algorithm lol!
- To iterate over all the Elements in an Array we can use [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) Array Loop.
2. Object Data Structure
- An **Object** is an **unordered** collection of key-value pairs. -> The Keys must either be a String or Symbol Data Type whereas the Values can be of any Data Types.
- To **access** a value or to **retrieve** a value we can use the corresponding Key -> this can be achieved using the Dot Notations or Bracket Notations.
  - Syntax example `obj['name']` or `obj.name`. -> **Constant Time Complexity O(1)**
- To **add** a **key-value** pairs this very same syntax is used to add key-value pairs to the Object.
  - Syntax example `obj['name']='Aleksandar'`. -> **Constant Time Complexity O(1)**
- To **delete** a key-value pair we can use [`delete`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete) operator.
  - Syntax example `delete obj['name']`. -> **Constant Time Complexity O(1)** -> since the `delete` keyword replaces the key-value pair with `undefined` (***or actually that was the case ONLY with Arrays***).
- Object property can be a function that is called a method.
- We can invoke/call a function from an Object's property -> That's also called a method invocations.
- Object Methods:
- [`Object.keys`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) returns an Array of Keys as its Elements. -> **Linear Time Complexity O(n)** -> iterates over all the enumerable Keys in the Object.
- [`Object.values`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values) returns an Array of Values as its Elements. -> **Linear Time Complexity O(n)** -> iterates over all the enumerable Keys in the Object.
- [`Object.entries`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) returns a nested Arrays inside of an Array which each nested Array holds 2 items **key-value** pairs respectively -> **Key** at index `0` (first item) and **Value** at index `1` (second item). -> **Linear Time Complexity O(n)** -> iterates over all the enumerable Keys in the Object.
  - (_I slightly modified the MDN's definition and I may submit a PRs contribution since theirs is a bit confusing._)
- Searching for a specific Value in an Object is **Linear Time Complexity O(n)** Worst Case Scenario we have to search over all the properties inside that Object.
3. Set Data Structure
- 1. Set is a Data Structure that can hold a collection of Values -> Values **must** be unique.
  2. Set can contain a mix of different Data Types -> we can store Strings, Booleans, Numbers or even Objects (or rather Objects Pointers).
  3. Sets are dynamically szed -> we don't have to declare the size of the Set before creating it.
  4. Sets do ~**not**~ maintain the insertion order -> meaning an Item Inserted First **doesn't** necessarily mean it's the first item in the Set.
    - Turns out he was wrong; as per in the comments && [MDN: iteration over a **Set** visits elements in **insertion order**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set#iterating_sets).
    - In the ECMAScript 2015 specification and later versions (**ES6+**), the **insertion order** is **maintained** in **Set objects**.
  6. Sets are iterables -> Set can be used within a [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) loop.
- (_Differences between Array and Set_) Sets VS Arrays:
  - Arrays **can** contain **duplicate** values whereas Sets **can't hold duplicate values** (_which can be a positive thing_).
  - Insertion order is maintained within Arrays but insertion order is ~**not**~ guaranteed with Sets.
    - Turns out he was wrong; as per in the comments && [MDN: iteration over a **Set** visits elements in **insertion order**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set#iterating_sets).
    - In the ECMAScript 2015 specification and later versions (**ES6+**), the **insertion order** is **maintained** in **Set objects**.
  - Searching and Deleting an Item in a **Set** is **faster** than Arrays.
- **Create a Set** Syntax example `const set = new Set([555, 888, 'Aleksandar'])`.
  - `for (const item of set) { console.log('item:', item) }`.
- **Add a new value** using [`.add`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/add) method **ONLY** if there isn't an item with the same value already in the Set -> meaning a **duplicate value** will be **ignored**. -> Average **Constant Time Complexity O(1)**.
  - However, in the **worst case scenario** where the **hash** function used by the Set has **poor distribution**, the time complexity can be **O(n)**, where `n` is the number of elements in the Set.
- **Check if a value exists** in the Set using [`.has`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/has) method `return`s `Boolean`. -> Average **Constant Time Complexity O(1)** as Set typically involves a **constant lookup time**.
  - In the worst case scenario, where there are many **collisions**, the time complexity can be **O(n)**.
- **Delete a value** from the Set using [`.delete`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/delete) method `return`s `Boolean`. -> Average **Constant Time Complexity O(1)**.
  - However, in the worst case scenario, where the **hash** function leads to a large number of **collisions**, the time complexity can be **O(n)**.
- **Check the amount of items** in the Set using [`.size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/size) method `return`s `Number` of items in the Set. -> **Constant Time Complexity O(1)** as the **`size` value** is usually **stored** as a **property** and **ONLY updated internally** whenever **elements** are **added** or **removed**.
- **Delete all values** from the Set using [`.clear`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/clear) method `return`s `undefined` and removes all the items from the Set Object. -> **Constant Time Complexity O(1).**
4. Map Data Structure
- [**The Map object holds key-value pairs and remembers the original insertion order of the keys. Any value (both Objects and Primitive Data Type Values) may be used as either a Key or a Value.**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
- It means that **Maps are Ordered** Objects OR collections of key-value pairs. (*While Objects are **unordered.***)
- Map's key-value pairs can both be of **any** Data Type.
- To **access a value** or to **retrieve a value** we can use the corresponding Key.
- Maps are iterable -> Map can be used within a [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) loop.
- (Differences between Object and Map) Maps vs Objects:
- Objects are **unordered** whereas Maps are **ordered.**
- Keys in Objects can only be String or Symbol data type whereas in Maps Keys can be of **any** Data Type.
- An Object has a **prototype** and may contain a few default Keys which may collide with our own Keys if we're **not** careful. VS Maps on the other hand does **not** contain any Keys by default.
- Objects are **not** iterables whereas Maps are **iterables**.
- The number of items in an Object must be determined manually whereas it is readily available with the **size** property in a Map
  - Wait he [said](https://youtu.be/XOpKmpRh69Y?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=80) that Map does **not** contain any Keys by default so how does Map contains `size` key AKA `size` property **now** all of a sudden? (***Possible mistakes by video creator @codevolution.***)
- Apart from storing data we can attach functionality to an Object whereas Maps are **restricted** to **storing data only.**
- **Create** a Map Object syntax `const map = new Map([['name', 'Aleksandar'], ['points', 888]])`.
  - This Syntax is very similar with the Nested Arrays represent **key-value pairs** much like what `Object.entries` `return`s when used on Objects Data Structure.
    - Nested Arrays [**must** be of `length` **2**](https://youtu.be/XOpKmpRh69Y?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=195).
      - Otherwise I tested I've added 3rd item and the 3rd item is being ignored **no** Errors **nor** issues so that's fine.
- `for...of` loop on Map syntax:
```js
const map = new Map([['name', 'Aleksandar'], ['points', 888]]);

for (const [key, value] of map) {
  console.log(`${key}: ${value}`);
};
```
- **Add a new** key-value pair in the Set using [`.set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/set) method `return`s the `Map` object itself. -> **Linear Time Complexity O(1)**.
  - Time Complexity might degrade to **O(n)** if there are many collisions in the **hash table** Data Structure (*I guess* -> This is because resolving collisions may involve traversing a chain or list of elements with the same index.).
- **Check** if a Key exist in the Map using [`.has`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/has) method `return`s `Boolean` value. -> **Linear Time Complexity O(1)**.
  - Time Complexity might degrade to **O(n)** if there are many collisions in the **hash table** Data Structure (*I guess* -> This is because resolving collisions may involve traversing a chain or list of elements with the same index.).
- **Delete** a key-value pair from the Map using [`.delete`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/has) method `return`s `Boolean` value. -> **Linear Time Complexity O(1)**.
  - Time Complexity might degrade to **O(n)** if there are many collisions in the **hash table** Data Structure (*I guess* -> This is because resolving collisions may involve traversing a chain or list of elements with the same index.).
- **Check the amount of** key-value pairs in the Map using [`.size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/size) method `return`s `Number` of key-value pairs in the Map. -> **Linear Time Complexity O(1)** as the **`size` value** is usually **stored** as a **property** and **ONLY updated internally** whenever **key-value pairs** are **added** or **removed**. -> therefore `size` does not involve any iteration or travesal on the key-value pairs stored in the Maps.
- **Delete all** the key-value pairs in the Map using [`clear`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/clear) method `return`s `undefined` and removes all the key-value pairs in the Map. -> **Linear Time Complexity O(1)**.
  - **Key-value pairs** are called **elements** or **items** in MDN are **referred** to as the **key-value pairs.**
5. Stack Data Structure
- The Stack Data Strucutre is a Sequential Collection of items that follows the **LIFO** Principle of Last In First Out.
- The **last** item **inserted** into the stack is **first** item to be **removed**.
- **ANALOGY** Stack of Plates: The **last** plate placed on **top** of the **stack** is also the **first** plate **removed** from the stack.
- Stack is an abstract Data Type. It is **defined** by its **behavior** rather than being a **mathematical** model.
- The Stack Data Structure supports two main operations:
  1. **`Push`** which **adds** an item to the collection.
  2. **`Pop`** which **removes** the most **recently** added item from the collection.
- Stack Usage / Stack Data Structure Use Cases:
 - Browser history tracking.
 - Undo operation when typing (sytanx `CTRL + Z`).
 - Expression conversions such as infix to postfix (I don't understand these new programming terms).
 - Call Stack in JavaScript Runtime Environment (/ *CallStack JavaScript Runtime Environment*).
#### Implementating Stack Data Structure Implementation
- Stack **must** support at least those **2** operations methods:
  - `push(item)` - **Add** an item to the **top** of the Stack. -> Constant Time Complexity **O(1)** ([6:30:](https://youtu.be/SbjATifB2M8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=390)).
  - `pop()` - **Remove** the **top most** item from the Stack. -> Constant Time Complexity **O(1)** ([6:30:](https://youtu.be/SbjATifB2M8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=390)).
- Apart from these 2 operations we need to implement a few more operations:
  - `peek()` - **Get** the value of the **top item** **without** **removing** it.
  - `isEmpty()` - **Check** if the Stack is **empty**.
  - `size()` - **Get** the **amount** of items in the Stack (*`return`s number of items in the Stack*).
  - `print()` - **Visualize** the items in the Stack.
- I'm personally confused about [**Stack Array**](https://replit.com/@Codevolution/JavaScript-Data-Structures#stack-array.js) VS **[Stack Object](https://replit.com/@Codevolution/JavaScript-Data-Structures#stack-object.js)** Data Structure both are on repl.it -> but I just can't differentiate them nor does the video by @codevolution explains the differences rather **only** talks about Stack Arrays huh?!?
  - So! Stack Array is still made up of Array Literals `[]` under the hood and using the `Class` keywords in the overal implementation (*behind the scenes it is using function prototype inheritance*).
  - [6:06](https://youtu.be/SbjATifB2M8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=366) Stack vs Array:
    - An Array is an indexed List that allows **random read and write operations** *whereas* a Stack implements the LIFO pricinples
    - Insertion `.push` method and `.pop` method Time Complexity (1) Constant VS Array have Linear O(n) if you choose to insert or remove an item from index `0`.
##### IMPLEMENTATION CODE STACK ARRAY DATA STRUCTURE ([replit / repl.it](https://replit.com/@Codevolution/JavaScript-Data-Structures#stack-array.js)):
```js
class Stack {
  constructor() {
    this.items = [];
  }

  push(element) {
    this.items.push(element);
  }

  pop() {
    return this.items.pop();
  }

  peek() {
    return this.items[this.items.length - 1];
  }

  isEmpty() {
    return this.items.length === 0;
  }

  size() {
    return this.items.length;
  }

  print() {
    console.log(this.items.toString());
  }
}

const stack = new Stack(); // create new Stack
console.log(stack.isEmpty()); // check if Stack is Empty
stack.push(20);
stack.push(10);
stack.push(30); // add 3 items onto the Stack
console.log(stack.size()); // check number of items in a Stack
stack.print(); // Print the items in the Stack
console.log(stack.pop()); // Removes an item from the end of the Stack
console.log(stack.peek()); // Print the last item on TOP of the Stack. (should be 10 because 30 has been POPped off.)
stack.print(); // Print all the items in the Stack again.
```
##### IMPLEMENTATION CODE STACK ARRAY DATA STRUCTURE ([replit / repl.it](https://replit.com/@Codevolution/JavaScript-Data-Structures#stack-array.js))
```js
class Stack {
  constructor() {
    this.items = [];
  };

  push(element) {
    this.items.push(element);
  };

  pop() {
    return this.items.pop();
  };

  peek() {
    return this.items[this.items.length - 1];
  };

  isEmpty() {
    return this.items.length === 0;
  };

  size() {
    return this.items.length;
  };

  print() {
    console.log(this.items.toString());
  };
};

const stack = new Stack();
console.log(stack.isEmpty());
stack.push(20);
stack.push(10);
stack.push(30);
console.log(stack.size());
stack.print();
console.log(stack.pop());
console.log(stack.peek());
stack.print();
```
6. Queue Data Structure
- The Queue Data Structure is a Sequential collection of items that follow the principle of FIFO First In First Out.
- The first item inserted into the Queue is first item to be removed.
- A Queue of people -> people enter the Queue at one end (**rear/tail**) and leave the Queue from the other end (**Front/head**). (*Aha-moments for me.*)
- Queue is an abstract Data Type. It is defined by its behavior rather than being a mathematical model.
- The Queue Data Structure supports 2 main operations:
  1. Enqueue, which **adds** an item to the **rear/tail** of the collection
  2. Dequeue, which **removes** an item from the **front/head** of the collection.
  - [Visual explanation by @codevolution YouTube Video](https://youtu.be/ex8EHl5fq1o?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=79).
- Queue usages when we have to process in an orderly fashion, for example:
  - Printers trying to print multiple documents.
  - CPU task scheduling.
  - Callback Queue in JavaScript runtime environment to determine the order in which our code executes.
##### Queue Implementation Breakdown
- Queue must support at least 2 operations:
  - `enqueue(item)`, which **adds** an item to the **rear/tail** of the collection
  - `dequeue()`, which **removes** an item from the **front/head** of the collection.
- And a few more operations / queue methods:
  - `peek()` - **get** the value of the item at the front of the Queue without removing it.
  - `isEmpty()` - **check** if the Queue is empty.
  - `size()` - **get** the number of items in the queue (*count the total amount of items in the Queue*).
  - `print()` - **visualize** the items in the Queue.
##### Queue Array Implementation Code-Wise
- Queue Array or Array Queue is still made up for Array literal `[]`.
```js
class Queue {
  constructor() {
    this.items = [];
  }

  enqueue(element) {
    this.items.push(element);
  }

  dequeue() {
    return this.items.shift();
  }

  peek() {
    if (!this.isEmpty()) {
      return this.items[0];
    }
    return null;
  }

  isEmpty() {
    return this.items.length === 0;
  }

  size() {
    return this.items.length;
  }

  print() {
    console.log(this.items.toString());
  }
}

const queue = new Queue();
console.log(queue.isEmpty());
queue.enqueue(10);
queue.enqueue(20);
queue.enqueue(30);
console.log(queue.size());
queue.print();
console.log(queue.dequeue());
console.log(queue.peek());
queue.print();
```
##### Object Queue or Queue Object Implementation Code
- `if(!item) return;` -> this is the line to fix the bugs if AND/OR when when `queue.dequeue()` is called without an argument (*meaning @codevolution has [bugs](https://www.youtube.com/watch?v=ba15sgOiAOg&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=45) in his explanations.).
- Additionally, as per the concerns in the comments, I had added a private reset method `resetIndices` that resets the `head` and `tail` to `0` if the queue ever completely empties.
```js
class Queue {
  constructor() {
    this.items = {};
    this.front = 0;
    this.rear = 0;
  };

  enqueue(element) {
    this.items[this.rear] = element;
    this.rear++;
  };

  dequeue() {
    const item = this.items[this.front];
    if(!item) return; // this is the line to correct the bugs if when when "queue.dequeue()".
    delete this.items[this.front];
    this.front++;
    
    if (this.isEmpty()) {
     this.resetIndices(); // Call the resetIndices method if the queue becomes completely empty
    };
    
    return item;
  };

  peek() {
    if (!this.isEmpty()) {
      console.log("Queue is empty");
    };
    return this.items[this.front];
  };

  size() {
    return this.rear - this.front;
  };

  isEmpty() {
    return this.rear - this.front === 0;
  };

  print() {
    if (!this.isEmpty()) {
      console.log("Queue is empty");
    };
    console.log(this.items);
  };
  
  resetIndices() {
    this.front = 0;
    this.rear = 0;
  };
}

const queue = new Queue();
console.log(queue.isEmpty());
queue.enqueue(10);
queue.enqueue(20);
queue.enqueue(30);
console.log(queue.size());
queue.print();
console.log(queue.dequeue());
console.log(queue.peek());
console.log(queue.isEmpty());
queue.print();
```
7. Circular Queue Data Structure
- Circular Queue is an extended version of a regular Queue.
- The size of the Queue is fixed and a single block of memory is used as if the first item is connected to the last item.
- Circular Queue also referred to as **Circular Buffer** or **Ring Buffer** also follows the **FIFO** principle.
- A Ciruclar Queue will **reuse** the **empty block** created **during** the **dequeue** operation.
- When working with Queues of fixed maximum size, a Circular Queue is a great implementation choice.
- The Circular Queue Data Structure supported 2 main operations:
  - Enqueue which **adds** an item to the **rear/tail** of the collection.
  - Dequeue which **removes** an item from the **front/head** of the collection
    - Again so **tail** definitions is the **back** of the collection, and **head** definitions is the **front** of the collection.
- Circular Queue Visualization [video](https://youtu.be/ngNJps_RUg8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=69) by @codevolution on YouTube.
- Circular Queue Usage:
  1. Clock
  2. Streaming Data to act as a Buffer (*lookup Buffer definition*).
  3. Traffic lights.
##### 7.1 One way of Circular Queue Implementation (*below will be a shorter way yet still same functionality*)
```js
class CircularQueue {
  constructor(capacity) {
    this.items = new Array(capacity);
    this.rear = -1;
    this.front = -1;
    this.currentLength = 0;
    this.capacity = capacity;
  }

  isFull() {
    return this.currentLength === this.capacity;
  }

  isEmpty() {
    return this.currentLength === 0;
  }

  size() {
    return this.currentLength;
  }

  enqueue(item) {
    if (!this.isFull()) {
      this.rear = (this.rear + 1) % this.capacity;
      this.items[this.rear] = item;
      this.currentLength += 1;
      if (this.front === -1) {
        this.front = this.rear;
      }
    }
  }

  dequeue() {
    if (this.isEmpty()) {
      return null;
    }
    const item = this.items[this.front];
    this.items[this.front] = null;
    this.front = (this.front + 1) % this.capacity;
    this.currentLength -= 1;
    if (this.isEmpty()) {
      this.front = -1;
      this.rear = -1;
    }
    return item;
  }

  peek() {
    if (!this.isEmpty()) {
      return this.items[this.front];
    }
    return null;
  }

  print() {
    if (this.isEmpty()) {
      console.log("Queue is empty");
    } else {
      let i;
      let str = "";
      for (i = this.front; i !== this.rear; i = (i + 1) % this.capacity) {
        str += this.items[i] + " ";
      }
      str += this.items[i];
      console.log(str);
    }
  }
}

const queue = new CircularQueue(5);
console.log(queue.isEmpty());
queue.enqueue(10);
queue.enqueue(20);
queue.enqueue(30);
queue.enqueue(40);
queue.enqueue(50);
console.log(queue.size());
queue.print();
console.log(queue.isFull());
console.log(queue.dequeue());
console.log(queue.peek());
queue.print();
queue.enqueue(60);
queue.print();
```
##### 7.2 Second way _- as per in the comments and not my own code because I'm still learning -_ there's a much shorter code for Circular Queue Data Structure.
- I don't understand the parts that uses `% this.size` modulus calculations? Needs revisit/review.
```js
class CircularQueue{
    constructor(size){ // "size" or "capacity" is just a naming conventions
        this.items = new Array(size)
        this.size = size
        this.currentLength = 0
        this.rear = 0
        this.front = 0
    }
    isFull(){
        return this.currentLength === this.size
    }
    isEmpty(){
        return this.currentLength === 0
    }
    enqueue(element){
        if(!this.isFull()){
            this.items[this.rear] = element
            this.currentLength++
            this.rear = (this.rear + 1) % this.size
        }
    }
    dequeue(){
        if(this.isEmpty()){
            return null;
        }
        const item = this.items[this.front]
        this.items[this.front] = null
        this.front = (this.front + 1) % this.size
        this.currentLength--
        if(this.isEmpty()){
            this.front = 0
        }
        return item;
    }
    peek(){
        return this.items[this.front]
    }
}
```
#### EXPLANATIONS of the V2:
- Since I don't understand the parts that uses `% this.size` modulus calculations? -> There's this ChatGPT explanations:
Let's say we have a circular queue with a capacity of 5, represented by an array with indices 0 to 4. The `this.size` is set to 5 to match the capacity of the queue.

Initially, the `rear` and `front` indices are both set to `-1`.
When we enqueue an element, the `rear` index is incremented by 1: `this.rear = (this.rear + 1) % this.size`.
- When `this.rear` reaches `4` (the maximum index), the modulo operation will be `4 % 5`, which results in `4`.
- When `this.rear` exceeds `4` and becomes `5`, the modulo operation will be `5 % 5`, which results in `0`.
- This effectively wraps the `rear` index back to the beginning of the array, creating a circular behavior.
Similarly, when we dequeue an element, the `front` index is incremented by 1: `this.front = (this.front + 1) % this.size`.
- When `this.front` reaches `4` (the maximum index), the modulo operation will be `4 % 5`, which results in `4`.
- When `this.front` exceeds `4` and becomes `5`, the modulo operation will be `5 % 5`, which results in `0`.
- This wraps the `front` index back to the beginning of the array, maintaining the circular behavior.

By using the modulus operator `%` with `this.size`, the indices will always stay within the range of the array, ensuring that the circular behavior of the queue is maintained. It allows the indices to effectively wrap around to the beginning of the array when they reach the end, avoiding any index out-of-bounds errors.

8. **Linked List Data Structure**
### **NOTES 13 June 2023:** I DO UNDERSTAND THE CODE BUT I DON'T UNDERSTAND THE GOAL OF THIS ALL? WHY'D I NEED TO MEMORIZE IT ALL? 
#### (*I do understand some libraries that I'm using do use these stuff under the hood, but like in my case I've never encountered the need to use it and now I have to memorize it because, there's no "pain" that's pushing me to learn this since I've never encountered the needs of these Custom Data Structures. -> The question "why do I need these?" is the Spanish village while the code is easy to grasp.)
- A Linked List is a linear Data Structure that includes a series of connected **nodes**.
- Each **node** consists of a **data value** and a **pointer** that **points** to the **next node**.
- **ADVANTAGES OF LINKED LIST OVER ARRAYS:** The list items can be easily inserted or removed without reallocation or reorganization of the entire structure.
- **DOWNSIDES OF LINKED LIST:** Random access of items is not feasible and **accessing** an item has Linear Time Complexity **O(n)**.
- The linked list Data Structure supports **3 main operations:**
  1. Insertion - to **add** an item at the beginning or end or at a given index in the list.
  2. Deletion - to **remove** an item given its index or value.
  3. Search - to **find** an item given its value.
- Visual Linked List explanation [YouTube video](https://youtu.be/3OsxH-huRc4?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=83) by @codevolution.
- Linked List Usage:
  - All applications of both **Stacks** and **Queues** are applications _of_ Linked Lists Data Structure.
    - Here I'm not quite sure if it means that Linked List is made up of Stacks and Queues (*or can be made up of*) OR if ~Linked List is made up Stacks and Queues~ -> **NOPE** -> [1:50](https://youtu.be/3OsxH-huRc4?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=111) Linked List can be **used** to **implement Stacks** and **Queues**.
  - Image viewer (real world example: you can look at photos continuously in a slideshow
  - Linked List is asked in asked in interviews (*frequent interview questions*).
#### Linked List Implementation.
- EXPLANATIONS:
##### `prepend` operation: Constant Time Complexity **O(1)**
  - Linked List **PREPEND** EMPTY LIST ([source](https://youtu.be/NAPQ0ua02CA?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=104)) #1 if a lists is **Empty**->then we make HEAD to point at the newly created **NODE** meaning the very first item in the list!
  - #2 2:40 Linked List **PREPEND** a list when it's **not** Empty (meaning Existing list); **HEAD** points the first **node** AND **last** **node** points at **null*.
##### `print` operation:
  - ([source](https://youtu.be/A-9tzPuE1eA?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=116)) How do we know if we have covered **all** the **nodes** in a **list**? -> Once all the **nodes** have been covered then **current** pointer points at `null` and thus implies we have covered all the **nodes** in the list.
##### `append` operation: Linear Time Complexity **O(n)** *(we have `while` loop)*
- `append` method **adds** a new **node** at the end of the **list**.
- `append` method implementation in 3 steps ([source](https://youtu.be/3e6Xfnr5ME8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=13)).
	- **0:15 Step 1:** We have to create a new **node** that will be added to the list. -> The **node** will contain a value and a next pointer pointing at `null`.
	- 1:20 **Step 2:** If list is **empty list** we have to make the **head** pointing at the newly created **node** (*the very first **node** in the list*).
	- 2:10 **Step 3:** When the list is **not** empty. (*this is the `else` block as per 4:00*) -> Let's assume we start with a list that contains 3 **nodes**: **head** is pointing at the **first node**; and each consecutive is pointing at the **next node**; and the **last node** points at **`null`** -> so instead to **add** a new **node** at the **end** we must make the **last node in the list** to point at the newly created **node** in the list (*most recently added **node***).
		- The condition would be `previous.next !== null`. -> That will ensure the previous pointer lands at the very last **node** in the list. -> Once we **do** end up at the last **node** we make `previous.next` point at the new **node**. -> This will `append` the new **node** to the existing list.
	- 6.55: "Well it is possible to **`append`** a **new node** in **constant time** but that involves **maintaing** a **tail pointer** that **always poitns** at the **last node** in the **list**-> I will briefly talk about that later in the course, but for now I want you to understand the very **basic imlpementation of a linked list** with **just** the **head pointer**. -> For beginners starting with the simplest code possible is the goal.".
##### `insert` operation ([source](https://www.youtube.com/watch?v=Sd9Nps5nAdU&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=53)):
- `insert` method will **insert** a new **node** at a given **index** in the **list**. -> In this case the `insert` method will accept the **node** `value` as well as the `index` at which the **node** needs to be **inserted**.
- There's 3 scenarios:
	- 0:30 **Scenario 1:** when the `index` is **less** than `0` or **greater** than the `size` of the **list**. In such a scenario we simply `return;` from the function.
	- 1:40 **Scenario 2:** when the `index` is **equal** to `0` that is **inserting** the new **node** as the **first node** in the **list**. -> **Inserting** a new **node** at the beginning is the same as `prepend`ing -> since we already have a **`prepend` method** for that we can **reuse** it (*following DRY principle*).
		- 2:25 `prepend` method will take care of adding the **node** either to an **empty** list or an **existing** list. -> make sure to not *increment* the `size` here (**"here**: the `if (index === 0)` condition inside of the `insert` method) as that is handled by the `prepend` method itself.
	- 2:45 **Scenario 3:** where `index` is **valid** and **greater** than `0`.
		- 3:00 Visual explanation.
		- Similar to Arrays we have to treat them so that nodes are positioned `0`, `1` and `2`.
		- Let's say we need to insert a **new node** at index `2`. -> That is in **between** the **nodes** positioned currently `1` and `2`.
		- -> What we have to do is: make the **new** node to point to the node that node `1` is pointing at & change the **next pointer** from node `1` to the **new node** -> to write this logic requires a bit more udnerstanding -> advanced topics.
```js
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
    this.size = 0;
  }

  isEmpty() {
    return this.size === 0;
  }

  getSize() {
    return this.size;
  }

	// Constant Time Complexity O(1)
  prepend(value) {
    const node = new Node(value);
    if (this.isEmpty()) {
      this.head = node;
    } else {
      node.next = this.head;
      this.head = node;
    }
    this.size++;
  }

	// Linear Time Complexity O(n)
  append(value) {
    const node = new Node(value);
    if (this.isEmpty()) {
      this.head = node;
    } else {
      let curr = this.head;
      while (curr.next) {
        curr = curr.next;
      };
      curr.next = node;
    };
    this.size++;
  };

  insert(value, index) {
    if (index < 0 || index > this.size) {
      return;
    }
    if (index === 0) {
      this.prepend(value);
    } else {
      const node = new Node(value);
      let prev = this.head;
      for (let i = 0; i < index - 1; i++) {
        prev = prev.next;
      };
      node.next = prev.next;
      prev.next = node;
      this.size++;
    };
  };

  removeFrom(index) {
    if (index < 0 || index >= this.size) {
      return null;
    }
    let removedNode;
    if (index === 0) {
      removedNode = this.head;
      this.head = this.head.next;
    } else {
      let prev = this.head;
      for (let i = 0; i < index - 1; i++) {
        prev = prev.next;
      }
      removedNode = prev.next;
      prev.next = removedNode.next;
    }
    this.size--;
    return removedNode.value;
  }

  removeValue(value) {
    if (this.isEmpty()) {
      return null;
    }
    if (this.head.value === value) {
      this.head = this.head.next;
      this.size--;
      return value;
    } else {
      let prev = this.head;
      while (prev.next && prev.next.value !== value) {
        prev = prev.next;
      }
      if (prev.next) {
        removedNode = prev.next;
        prev.next = removedNode.next;
        this.size--;
        return value;
      }
      return null;
    }
  }

  search(value) {
    if (this.isEmpty()) {
      return -1;
    }
    let i = 0;
    let curr = this.head;
    while (curr) {
      if (curr.value === value) {
        return i;
      }
      curr = curr.next;
      i++;
    }
    return -1;
  }

  reverse() {
    let prev = null;
    let curr = this.head;
    while (curr) {
      let next = curr.next;
      curr.next = prev;
      prev = curr;
      curr = next;
    }
    this.head = prev;
  }

  print() {
    if (this.isEmpty()) {
      console.log("List is empty");
    } else {
      let curr = this.head;
      let listValues = "";
      while (curr) {
      // This While Loop will repeat until `curr` points after the last node === null -> Loop's Base Case & Exits.
        listValues += `${curr.value}->`;
        curr = curr.next;
      };
      console.log(listValues);
    };
  };
};

const l = new LinkedList();

console.log(l.isEmpty());
l.append(50);
l.prepend(20);
l.append(80);
l.insert(60, 2);
console.log(l.getSize());
l.print();
l.reverse();
l.print();
console.log(l.search(60));
l.removeFrom(4);
console.log(l.getSize());
l.print();
l.removeValue(80);
l.print();
console.log(l.getSize());
l.print();
```
9. Linked List with Tail Data Structure
- Has Head Pointers & Tail Pointers. 
- NOTES:
- There's Linked List Stack; Linked List Queue; Doubly Linked List.
10. Hash Table Data Structure
- NOTES:
- Hash Table Collisions
11. Tree Data Structure

12. Binary Search Tree Data Structure

13. Graph Data Structure
- NOTES:
- There's multiple exercises Adjacency Matrix of a Graph; Adjacency List of a Graph; Graph Add Vertex and Edge; Graph Display and HasEdge; Graph Remove Edge and Vortex.
---
### DEFINITIONS & EXTRAS
- **Traverse** definition: In programming, _Array traversing_ or to **traverse** typically refers to the act of iterating or moving through a data structure, such as an array, list, tree, or graph, in order to access or process its elements. -> Traversing a data structure involves visiting each element or node in a systematic manner according to a specific order or pattern.
- Algorithm Design Techniques Variety by @codevolution at [4:00](https://youtu.be/tCvSDnRsGnw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=240) YouTube Video.
  - Brute Force
  - Greedy
  - Divide and Conquer
  - Dynamic Programming
  - Backtracking
  - etc. (A few more exist but these are most important.)
---
My most helpful JavaScript based DSA course on YouTube by @Codevolution:
#### https://www.youtube.com/playlist?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP
NOTE#1: There's mistake in his Fibonacci Sequence so I'm fixing it on my own. His mistakes are both in his video: https://www.youtube.com/watch?v=tQjd29Rmo_A&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=7 where his `for` loop has a wrong condition of `i < n` instead of `i <= n` like his correct verison at https://replit.com/@Codevolution/JavaScript-Algorithms#math/fibonacci.js -> which however he kept the *wrong comments* but the code is fixed otherwise.
  - As well as Recursive Fibonacci Sequence mistakes: https://youtu.be/wZNxLwqxu00?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=55 here he correctly shows expected returned value BUT at https://replit.com/@Codevolution/JavaScript-Algorithms#math/fibonacci-recursive.js -> he mistakenly changed argument of 3rd call from `6` to `7` but wrong expected result `8` in a comment instead should be `13`, because result `8` corresponds to Fibonacci of `6`.

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
```js
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
```js
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
#### Queue Array or Array Queue Implementation Code-Wise
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
#### Queue Object or Object Queue Implementation Code
##### Fixing bugs as per in the comments (*which turned out to not really be a bug, #2 was concerns, and #1 was a typo on commenteer part*):
- #1 `if(!item) return;` -> this is the line to fix the bugs if AND/OR when when `queue.dequeue()` is called ~without an argument~ (*meaning @codevolution has [bugs](https://www.youtube.com/watch?v=ba15sgOiAOg&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=45) in his explanations*.).
  - That was my misconception about it, `dequeue` is always called without an Argument, because it removes from the *Front* AKA the very first item added AKA the item at index `0` -> AND if no item exists at `items[front]`: meaning it's empty (*I guess that's the only way when it doesn't exist*) we'll `return;` AKA exiting the function executions. But, however I could have used `isEmpty` method to begin it - at the top scope inside `dequeue` - as a condition of `if (isEmpty())`, but yeah I'll just keep the current implementation as-is.
    - Which I'm not entirely sure if condition `if (!item)` technically means the same condition as `if (this.isEmpty())`; at which point the second condition will never be reached inside `dequeue` method; that would mean I need to use the logic of `resetIndices` method or call it inside `if (!item)` since currently as-is the `dequeue` still won't reset `front` and `rear` both to `0`.
      - And looking at it I'm also confused why'd I need 1 call of `dequeue` on an already-empty Queue in order to set `front` and `rear` to `0`; it seem like way-too-late thing to do, as if I had to do it way before it -> so that the condition could have been `if (front === 0 && rear === 0)`.
  - I asked  ChatGPT for `if (!item)`: "If item is **falsy**, it means that the queue is **empty**, and there are **no** items to dequeue."
  - And also: "`isEmpty` method checks if the difference between the `rear` and `front` indices is **zero**, indicating an **empty** queue."
  - Which by me: it means that indeed the first condition is enough to handle an empty Queue, but I'm not entirely sure if I still need to set both `rear` and `front` to `0` so that I can technically "empty out the Queue" AKA to be garbage collected? But still! The main issue remains in the fact that I'll be setting them to `0` **only once `dequeue` is called on an already empty Queue` -> but that is so that I avoid checking if Queue is empty on every call because .. wait but that won't increase the Time Complexity to Linear O(n), since `isEmpty` method is O(1) Constant Time Complexity.
- Wait wait wait, for one: the dude in the comment had a typo, and when I better understand the comment that condition is useless when `item = this.items[this.front]` is not found then the `item` is set to `undefined` and: `return item` returns `undefined` -> The same thing he is doing with `if (!item) return;` is as same as `if (!undefined) return;` -> which `!undefined` is `true` -> *but* since `item` is already `undefined` the condition function `dequeue` would continue to `return item;` which would technically mean I'm already returning the same that - and that is `return undefined;`. -> ***This is why I'll remove the `if (!item)` condition below the code inside `dequeue` but keep the `if (this.isEmpty())`.*** -> **But, `if (this.isEmpty())` MUST be moved above or on top-level, so to make sure that `this.front++` is never even reached, but then I'll need a hanging `return;` *again* inside the `if (this.isEmpty())` condition.**
- #2 Additionally, as per the concerns in the another comment, I have added a private reset method `resetIndices` that resets the `head` and `tail` to `0` if the queue ever completely empties.
  - Looking back at this this didn't needed additional method `resetIndices` helper function is rather so short & never re-used, but yeah I'll keep the method.
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
    if (this.isEmpty()) {
      this.resetIndices(); // Call the resetIndices method if the queue becomes completely empty
      return; // Exit and return undefined (since no removal can be done on an Empty Queue).
    };
    
    const item = this.items[this.front];
    
    // Condition "if (!item)" is unnecessary, instead I'll keep the `this.isEmpty()` condition.
    // if (!item) return; // this is the line to correct the bugs if when "queue.dequeue()".
    
    delete this.items[this.front];
    this.front++;

    // // Instead I'll move this way above on top, so that "this.front++" is never even reached.
    // if (this.isEmpty()) {
    //  this.resetIndices(); // Call the resetIndices method if the queue becomes completely empty
    // };
    
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
  - But by me: this.front+1 will never be above the this.size; so lets say 0+1=2 -> 2%5=2 modulus.

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
		- -> What we have to do is: make the **new** node to point to the node that node `1` is pointing at & change the **next pointer** from node `1` to the **new node** -> to write this logic requires a bit more understanding -> advanced topics.
##### `removeFrom` operation ([source](https://youtu.be/D_kWagEfcx8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=16)):
- 0:12 `removeFrom` method will be a **removal** implementation from a **given index** as well across 3 scenarios:
	- 0:20 **Scenario 1**: `index` is **less** than `0` or **greater** than the `size` of the **list**. -> In such a case we return `0` as **no** item can be removed.
		- 0:55 condition is `if (index<0)` inside `removeFrom` method.
	- 1:30 **Scenario 2:** `index` is `===` to `0` -> that is removing the **head node** which is the **first node** in the **list**. Removing the **node** involves pointing **head** at the second node in the list. -> In terms of code: **head** would point to its own next pointer which always points at the second node in the **list**. The first **node** is detach and garbage collected (*oh the old forgotten terms garbage collectors*) -> **head** will continue to point at the first node in the new list which is the expected ehavior.
		- 2:10 if the list has only 1 node to begin with -> head will still point at its own-next pointer but that will just be `null`. -> if **head** is pointing at `null` then it indicates empty list!
		- Code wise that is the variable `removedNode` that will hold a reference to the **node that will be removed**.
		- Next we check if `index === 0` -> if yes then we'll store a reference to the **head** node in the `removedNode` variable to the `this.head`. We then change the head **node** from the first node to the second **node**.
		- Next `this.head` = `this.head.next` -> if the list contains only 1 **node** -> **head** will now **point** at `null`.
		- `this.size--` & `return removedNode.value` this code now takes care of invalid index and removing a node from the beginning of the list.
	- 3:40 **Scenario 3**: `index` is valid AND greater `>` than `0`.
	- (In meantime reading the comment "*you may delete your tail node, what do you do in that case?*" -> answer seems to be to modify the `for (let i = 0; i < index - 1; i++)` code into `for (let i = 0; i < this.size; i++)`.)
		- We're going to treat the `index` similar to Arrays so the **nodes** are positioned at index `0`, `1` and `2` and `3`. -> if we need to remove the **node** at index `2` that's between `1` and `3` -> (4:14) the only **rule** we have to fulfill after deleting is that the nodes **must** point to the **next node** in the **right order.** - so we have to make the node at position `1` to point directly at position `3` -> that will effectievely detach node at position `2` from the list.
		- 4:44 A **rule reminders** (*rules reminders*) whenever we have to **do** something that is **not** at the **head** of the list it generally involves a **temporary** pointer that moves across the list.
		- Deeper Explanations (4:00):
		- To delete a node at a given index we need to get a hold of the node previous to that index. For example to delete a node at index `2` we need a reference to node at index `1`. For that purpose we're going to use a temporary pointer called `previous`. We will start off with `previous` pointing at the head node. We will then traverse the list advancing the `previous` pointer until we reach the node that is `previous` to the index we need to delete from. 
		- In our case example we need to delete from index `2` so we advanced the previous pointer until index `1`. At this point we will get hold of the node to be removed using `previous.next`. -> So, a new pointer called `removedNode` would be equal to `previous.next`. -> We then break the link from node at position `1` to node at position `1` and instead link node at position `1` to node at position `3`. That is point `previous.next` to `removedNode.next` -> Connect node `1` directly with node `3`. This will provide the continuity required. There is no way now to reach the removed node from the head and hence it will be garbage collected. As you can see **deletion** being similar to **insertion** is as **simple** as **changing** the **next pointer** of the individual nodes.
		- The code is in the `else` block of inside `removeFrom`.
##### `removeValue` operation ([source](https://youtu.be/RgrIXX3E2mY?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=26)):
- `removeValue` method logic (*cude breakdown*): it will **remove** the first node that contains the value. It will `return` the `value` `if` the **node** was **removed** or `else` `return` `null` `if` the **node was not found**.
- 0:25 Across 3 scenarios:
	- 0.28 **Scenario 1**: The first scenario is where the list is empty. In such a case we will return `null` as no node can be removed.
	- Let's call the method `removeValue` which accepts a `value` argument that a node might contain.
	- `if (this.isEmpty())` conditional code is the Scenario 1 -> then `return null` ensuring at least 1 node is present in the list which might contain a value equal to the pasted value/passed value and we can proceed with removing such a **node** from a list. It's welcome to add a message before `return`ing from the method for more clarity in the log statement (? - why he didn't added a message or comment ?).
	- 1:28 **Scenario 2:** where  **passed** **value** is equal to the value of the first node in the list: that is **removing** the head node. Removing the head node involves pointing head at the second node in the list. In terms of code head would point to the next poitner which always points in the second node in the list. The first node is  reachable and is garbage collected. Head will continue to point at the first node in the new list which is the expected behavior. If the list has only one node to begin with, head will still point at its own next pointer but that would just be `null`.  When you remove  the only node in the list  we would have an empty list.
		- Code `if (this.head.next === value)`.
		- Then we point **head** to its next pointer: `this.head = this.head.next`.
		- `return value` is a business logical decisions -> we can return either the `value` or a `message` saying that the **node** has been deleted - if we wanted more empathy in the business logic decisions.
	- 3:18 **Scenario 3:** where **value** is present in the **node** that is **not** the **head** OR the **value** is **not** present in the **list at all.**
		- 3:30 visual explanations of what it means to remove a node somewhere in the middle of the list:
		- Say we have 4 nodes in the nodes containing the values 10, 20, 25, 30, respectively.
		- Let's say we try to remove the value `25`.
		- The only rule we have to fulfill after deleting is that the noeds must point to the next node in the right order.
		- So, make the node of value `20` to point to node with value `30` (4th; bcuz 3rd (`25`) will be removed).
		- ([4:14](https://youtu.be/RgrIXX3E2mY?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=255) of YouTube video about `removeValue` and previous video at [4:44](https://youtu.be/D_kWagEfcx8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=284) about `removeFrom`) Again from previous 2 videos **RULES REMINDERS**(*a rule reminders)*: **if we wanna do something that is not at the head of the list it generally involves a temporary pointer that moves across the list**. 
		- -> And to delete a node that contains the given value, we need to get hold of the node previous of the node that contains the value.
		- For example to delete node `25` -> we **need reference** for node `20` value.
		- 4:44 For that purpose we're gonna use temporary pointer called `previous` & here's how:
		- We'll start at `previous` pointing at the `head` node.
		- 4:55 Then we'll **traverse** the list advancing the previous pointer until we reach the node that is `previous` to the node that contains value `25`.
		- The condition will be to advance `previous` for as long as there exists a next node in the list `&&` the next node does **not** contain the passed in value.
		- In our case we advance until node that contains `20` value, as the next node contains `25` which is the node we wanna remove.
		- At this point `previous.next` **points** at the node that must be deleted (`25` value) AND `removedNode` points to the **node** that should now be linked to the previous node.
		- 5:40 So all we have to do is change `previous.next` into `removednode.next`. This will provide the continuity required.
		- 5:55 Node `25` will be garbage collected and we'll ahve the new list with 3 nodes and `head` still pointing at the first node.
		- 6:06 **We have a case where the `value` may not be present in the list.**
		- Such a scenario if `value` passed into `removeValue` method is `60` (**which does not exist in our list**). In this case our `previous` pointer would have advanced into the last node in the list as it does **not** find that `value` in any of the nodes. At that point in time we check if there is a `next` node -> if there's **no** `next` node -> it simply means that we have **reached** the **end** of the list without finding the node that contains the passed in `value`. in that case we simply return `null`.
		- The code starts of inside `else` statement inside `removeValue` method:
		- Initialize `previous` pointer to head: `let prev = this.head;`.
		- Next we advance the `previous` pointer as long as there is a `next` as long as there is a `next` node in the list and that node does not contain the passed in `value` argument; code: `while (prev.next && prev.next.value !== value)` -> `prev = prev.next;`
		- 7:47 When `while` loop exits; 1 of **2 things** are possible:
			- #1 The `previous` pointer has **stopped** at the node `previous` to the node which has to be removed. -> So there does exists a **node-to-be-removed**; code: `if (prev.next)` -> we then store temporary variable as the **node-to-be-removed** in `removedNode = prev.next` equals to the node after the `previous` pointer (**`prev.next`**) AND we change `prev.next = removedNode.next`; 8:30 ALSO decrement the `size` as `this.size--` and `return value;` or `return` some message with empathy for the client / frontend user.
			- 8:40 #2 `else` `if` the `previous` pointer reached the last node in the list `&&` there is **no next node** we'll `return null` as **no node could be deleted.**
		- 10:50 Time Complexity: **Linear Time Complexity O(n):**
			- Big O Notations calculations ->: removing the **HEAD** **NODE** is **ALWAYS** Constant Time Complexity O(1) **HOWEVER** removing a node in general has **Linear Time Complexity O(n)** as the node to be **removed** might be the last node in the list. In certain articles you might find separate functions or methods to **remove head** and **remove node** separately, but in our implementations covers both.
- **BUGS IN THE IMPLEMENTATIONs as per the comments in the YouTube Video about [`removeValue` by @codevolution](https://www.youtube.com/watch?v=RgrIXX3E2mY&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=55):**
	- @Ali: "*What **if** we have to **remove** **last** value which has **`null`** in **next**. I have tried, but it **cannot** be **removed.***".
	- By me: Since I'm studying all of this and have no idea what's the purpose of Data Structure as I don't understand the goal for Data Structures yet, here's ChatGPT answer for a **fix**(take with grain of salt):
```js
// The code should be places inside `class LinkedList`
// Then inside `removeValue` ONLY before `return null`
if (prev.value === value) {
	this.head = null; // Set the head to null when removing the last value
	this.size--;
	return value;
};
```
- So,
	- Let's implement it inside the final code as well below!
##### `search` operation ([source](https://www.youtube.com/watch?v=ZRIJuAIGJ4M&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=56)):
- 0:10 `search` method implementation we will **return** the **first `index`** or position at which the `value` is present or `-1` if the `value` is **not** found in the list OR business logic: we can return the **node** itself (***by me that's a `find` method under the hood***).
- 0:25 `search` method implementation is very similar to the `print` method and falls under 2 categories:
- 0:33 **Scenario 1:** when the list is **empty**. -> IN such a case we return `-1` as **no** **node** can be found.
	- 0:55 To handle **scenario 1** we make use of `if` statement: the code of inside `search` method: `if (this.isEmpty())` condition -> then `return -1` -> we make sure that at least 1 node is present in the List which might continue a `value` equal to the passed in `value`. -> for business logic choices you can `return` a message instead.
- 1:30 **Scenario 2:** when the list is **not empty**. Very similarly to `print` method. However, instead of `console.log`ging the node `value` to the `console` we instead `return` the `index` of that node.
- 1:50 Here's how: we're gonna create a variable `i` to keep track of the node `index` and a temporary pointer `current` to **traverse the list**. -> At the beginning `current` will point at the **head node**.
	- 2:05 Then, we'll use the `next` pointer in **each node** to advance the `current` pointer and check `if` the **node** contains the `search`'s `value` -> `if` it does then we'll `return index` value (*AKA `return i` in the code below*).
	- 2:22 An example: trying to find a node with value `30` in a nodes of: **10, 20, 30, 40** values (*reminder by me: `40` will point at `null` but `null` is **not** a value, but it's just the **last node**'s **pointer**s*).
	- 2:28 That node of value `30` is at index `2` (*Linked List is `0` based index*) using the `current` pointer.
	- 2:40 `if` we reach a state where `current` pointer **points at `null`** (*AKA the last node of `40` value in this case example*) -> we've crossed the last **node** in the list and we did **not** encounter the `search` `value` argument then we'll `return -1`.
	- 3:00 The code `let i = 0;` `let curr = this.head`
	- 3:21 **Traverse definition: loop over the Linked List** -> in this case we'll traverse with `while` loop's condition `while (curr)` -> shortcut meaning: `curr !== null` -> then **compare** if the `curr` node `value` is `===` to the passed in `value` **argument** -> then `return index`
		- That explanation in code: `if (curr.value === value) { return i; }`.
	- 3:40 `else` is ommitted instead we continue the code flow: 3:48 **Advance the `current` pointer and increment the `index++`** (*again, in final code it's shortened to `i++`*):
		- `curr = curr.next; i++;`.
- 4:04 when `while` loop exits we can infer that the **node** was **not found in the list** and we'll `return -1` (*outside below the `while` loop*).
#### **REVERSE A LINKED LIST** ([source](https://www.youtube.com/watch?v=S9kMVEUg-x4&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=57)):
- Empathy: 0:15 code would **not** make any sense if you **don't** have the mental image of what happesn when you **reverse the Linked List**. Rather focus on the visual representation than the actual code ->
	- (*So, the downsides for my by me now **is that all the while** it's the understanding that matters and not memorizing it; on top of it all I dont really understand the use cases since I've never encountered the need for these Data Structures and that's what stopping me from creating a visual understanding rather than I'm trying to memorize it because I never had the pain o fneeding any of these solutions -> so i'm memorizing logical solutions because  I never had real experience where I struggled with it and I was in pain and needed to look up this Linked List Data Structure.*)
- Example the **logic breakdown**:
	- 0:40 Say we have a normal Linked List with **3 nodes**: **1, 2, 3**
	- `1` is the **head node** and points at `2` points at `3` points at `null`.
	- 0:50 **The Reversed Linked List** contains **3, 2, 1,**
	- `3` is the **head node** and points at `2` points at `1` points at `null`.
- 1:11 Here's how the code breakdown logic:
	- We're going to create **2 Temporary Pointers**: `previous` which does **not** point at any nodes OR technically it means that it points at `null`; AND `current` pointer points at **Head**.
- 1:30 For every **node** in the List we execute **4 Steps:**
	- 1:35 **Step 1:** We create a **new temporary pointer** called `next` and **point it to** `current.next`.
	- 1:50 **Step 2:** Node `2` is the **next node** in our example -> We then **set** `current.next = previous` -> meaning `1` now **points at** `null` instead of pointing at `2` -> the Direction has **reversed**.
	- 2:08 **Step 3:** We then update `previous` to `current` **meaning** `previous` now **points** at `1` AND `current` points at `next` **meaning** `current` points at `2`.
	- 2:28 We **repeated** the **same** operations for the 2nd and 3rd **node**s:
		- `next` points at `current.next`.
		- `current.next` points at `previous`.
		- `previous` points at `current`.
		- `current` points at `next`.
	- 2:55 **repeat** the same for the `3`rd **node** and we have the **REVERSED LINKED LIST!** -> now `3` pointing at `2` points at `1` points at `null`.
	- 3:10 One last **final** change though is to assign the `previous` node as the **head node**. -> **Remember** **head** should **always** point at first node in the List.
	- "*If this was slightly confusing please rewatch the past few minutes*" - No, I do understand it all I just don't memorize it because, again, I had never the emotional pain of needing to build such a Data Structure.
##### 3:30 (*of the same video*) The Code will implement `reverse` method:
- Code begins with `let prev = null; let curr = this.head;`.
- `while (curr)` condition has **4 steps** code explanations
	- 4:06 **Step 1** create temporary `next` pointer that **points** to the `next` **node** after `current`; code: `let next = curr.next`.
	- 4:22 **Step 2** make the `curr`ent node to **point in REVERSE**; the code: `curr.next = prev`.
	- 4:34 **Step 3** advance the `previous` pointer; code: `prev = curr`.
	- 4:40 **Step 4** advance the `curr`ent pointer; code: `curr = next`.
- This `while` loop **exits when** **all** the **node** in the List have been covered (*here he means all the nodes in the Linked List have been Traversed AKA Looped over*).
- 4:55 At this stage make sure to **set** **head** to the **new** first item in the List; code: `this.head = prev`ious.
### FINAL CODE LINKED LIST DATA STRUCTURE:
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
      return value; // either retirn "value" or an empathetic message saying that the node has been deleted.
    } else {
      let prev = this.head;
      while (prev.next && prev.next.value !== value) {
        prev = prev.next;
      }
      if (prev.next) {
        // removedNode = prev.next; // in Strict Mode this is a Reference Error "removedNode is not defined"
        const removedNode = prev.next; // const or let would both work in this case.
        prev.next = removedNode.next;
        this.size--;
        return value;
      }
      
      // ChatGPT fix for the comment above (read notes about `removeValue`):
			if (prev.value === value) {
				this.head = null; // Set the head to null when removing the last value
				this.size--;
				return value;
			};
			
      return null;
    };
  };

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
    };
    // Exiting the while loop means `curr.next` is now pointing at `null` -> reached the end;
    // at which point the `value` is not found.
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

const list = new LinkedList();

console.log(list.isEmpty());
list.append(50);
list.prepend(20);
list.append(80);
list.insert(60, 2);
console.log(list.getSize());
list.print();
list.reverse();
list.print();
console.log(list.search(60));
list.removeFrom(4);
console.log(list.getSize());
list.print();
list.removeValue(80);
list.print();
console.log(list.getSize());
list.print();
```
9. **Linked List with Tail Data Structure**
- Has Head Pointers & Tail Pointers.
- *(Also called Linked List with a Head and a Tail Pointers / Linked List with a Head Pointer and a Tail Pointers.)*
- NOTES:
- I've noticed there's Linked List Stack DS; Linked List Queue DS; Doubly Linked List DS
- Next,
##### Linked List with Tail Overview ([source](https://www.youtube.com/watch?v=sw7fCZeFTdc&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=58))
- In the past 10 Videos about Linked List!.. There has been 10 videos solely focused on this topic; and there's more..:
- 0:20 `prepend` method AKA **insertion** at the beginning of the list has a Constant Time COmplexity **O(1)** BUT `aappend` method AKA **insertion** at the end has Linear Time Complexity O(n) (Hint:**bad**) -> this can be improved by having to a Constant Time COmplexity (1) by keeping track of an additional pointer called **tail** which **always** points at the **last node** in the List.
- 0:50 This video is an Overview of how the Linked List Data Structure works with both the **Head** Pointer & **Tail** Pointer.
##### Case 1: When the List is **empty**:
- 1:11 When the List is **empty** and we have just the **head** pointer pointing at `null`.
- While with a **tail**: we have both the **head and tail** pointing at `null`.
##### Case 2: To `insert` first node in the List:
- 1:30 We'll create a **node** and point `head` at that **node**. 
- While with a **tail**: to `insert` the first **node** in the List -> we create a **node** and point point both `head` and `tail` at that **node**.
	- When there's only 1 **node** it is both the **first node and the last node**.
##### Case 3: To `prepend` a node:
- 1:50 TO `prepend` a node it is same in both the cases: Create a **new** **node**, point its `next` pointer at the **head node** AND point `head` at the **new node**.
##### Case 4: `append`ing is where there is a difference:
- 2:10 With just the **head node** to `append` a new node we obtain a reference to the last node AND point its `next` pointer at the new node.
- While with a **tail**: We already have a reference to the `last` node. -> All we have to do is **create a new node;** point `tail.next` to the **new node** AND assign the **newly created node as the Tail node for the List.** -> 2:40 In such case **insertion** in both ends has a Constant Time COmplexity **O(1).**
##### Case 5: `delete` only one node:
- 2:50 **Scenario 1:** when there's only 1 **node** in the List: deleting that node involves making **head** point at `null`. -> With a `tail` deleting that node involves making **both head and tail point at `null`**.
- 3:09 **Scenario 2:** if the List contains **more than 1 items** - deleting a **node** from the **start** is the same for both the cases: make **head** point to its **next node**. -> While deleting a **node from the end** is mostly similar: the only difference is that we have to **re-position** the **tail pointer**: so to **delete* a node in both the cases we obtain a reference to the node `previous` to the last node. Then,  we point that `previous` node to `null` **which effectively removes the  last node from the List (*reminder this is technially removing the  last node from the list***).
- 3:45 **Scenario 3:** with the **tail** pointer we update the tail pointer to point to that `previous` **node** which is the **new last node in the list.** -> 3:53 Deleting from the **start** has Constant Time Complexity **O(1)** VS deleting from the **end** has a Linear Time Complexity **O(n).**
##### IMPORTANT about Linked List with Tail Overview:
- 4:10 While `insert`ing and `remove` method: removing a node **in between** the **first** and **last** **nodes** AND `search`ing for a **node** in the **List** and `reverse` method: reversing the List is the **same in both the Cases *(with and without a Tail).***
##### Linked List with Tail Implementation of the `insert`ion and `remove` method ([source](https://www.youtube.com/watch?v=ADZfDxftXQ0&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=59)):
- By me: To begin with, there's a comment or multiple of comments pointing out at the **mistake** he made for the `reverse` method making the issue for `removeFromEnd()` "*and the fix is to add `this.tail = this.head;` **BEFORE** the `while` loop begins.*".
	- Comment stands: "*I also want to note if you kept the code from previous videos, that the reverse() method will cause removeFromEnd() to **crash.*".**
	- **NOTE** in repl.it @ https://replit.com/@Codevolution/JavaScript-Data-Structures#linked-list-tail.js -> that's **ALREADY SEEM TO HAVE BEEN FIXED inside `reverse` method** *(the mistake is inside the Video **only**)*.
- By me2: And another issues/mistakes/bugs with his code is with `removeFromFront`; and in the reply seciton the fix seems to be: "*`if (head === null)` condition then `tail = null;` AND the `return` should be removed from the above as it breaks the flow of code and save the `return`able value into a static variable which can be `return`ed at the end of all the conditional statements.*".
	- The comment explaining the issue stands: "*There is a bug in removeFromFront method, when there is one node in linked list and head and tail both point to it, the head will become `null` but the tail will still point to the same node, tail should also have been set to `null`*.".
	- Well if so, I have to make a code that would be a fix for this second bug because it hasn't been updated in his repl.it.
	- Of inside `removeFromFront` method *(at [repl.it](https://replit.com/@Codevolution/JavaScript-Data-Structures#linked-list-tail.js))*:
	- But there do I place it I have no idea if order matters, so I asked ChatGPT told me that I can even use `if (this.size === 1)` condition.
	- Since I don't understand I think such a condition may be an issue if the `size` doesn't matter in a case where there is a reversal traversal of removing an item? -> or wait, actually the method name is pretty clear `removeFromFront` so all we fix is the **logic removing a Node from Front** which now makes sense that th condition would work *(and I don't need 2 of them just 1 is **enough):***
```js
removeFromFront() {
	if (this.head === null || this.size === 1) { // Both Conditions does the same.
		this.tail = null;
	};
	//...
};
```
- 3rd Comment is a question about Big O Notations Calculations of `removeFromEnd` method and that seems to be of a Linear Time Complexity **O(n)** as the `while` loop is used inside of it.
- 0:40 `isEmpty`, `getSize` and `print` methods are **same as before**.
- 1:11 In [this video](https://www.youtube.com/watch?v=ADZfDxftXQ0&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=59) there will be 4 methods implemented for the Linked List with a Tail:
- `prepend(value)`.
- `append(value)`.
- `removeFromFront()`.
- `removeFromEnd()`.
- 1:20 Starting with `prepend` method & explanations will be shorter now (*yet video lasts 10 mins because he says:*) I will be showing you visual explanations after each method implemented.
- *(Well here I stop writing things down, as I do understand the logic behind the **CODE flow**, but I don't understand the real life use cases, so I consider this to be enough, as well as because @codevolution will keep repeating himself from the previous videos I already took very lengthy notes.)*
### FINAL CODE Linked List WITH A Tail CODE IMPLEMENTATIONS:
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
    this.tail = null;
    this.size = 0;
  }

  isEmpty() { // No changes in Linked List with a Tail IMPLEMENTATIONS
    return this.size === 0;
  }

  getSize() { // No changes in Linked List with a Tail IMPLEMENTATIONS
    return this.size;
  }

  prepend(value) {
    const node = new Node(value);
    if (this.isEmpty()) {
      this.head = node;
      this.tail = node;
    } else {
      // Unshifting the List technically:
      // by assigning NEXT to the this.head
      node.next = this.head;
      // and this.head becomes the new NODE
      this.head = node;
    };
    this.size++;
  }

  append(value) {
    const node = new Node(value);
    if (this.isEmpty()) {
      this.head = node;
      this.tail = node;
    } else {
      // Pushing a Node technically by assigning the 
      // what's-to-become-a-previous-to-the-(to-be-new)-last-node's NEXT to point at the new NODE
      this.tail.next = node;
      // and assigning Tail to the Newly Created NODE
      this.tail = node;
    };
    this.size++;
  }

  removeFromFront() {
    if (this.isEmpty()) {
      return null;
    }
    const value = this.head.value;
    this.head = this.head.next;
    // Fix for a bug when there's only 1 Node left in the List -> Tail should point at NULL:
    // (BTW I'm thinking maybe it should be moved above, but not really, this is rather a case where
    // `size > 0` -> and EXACTLY `size === 1` means: last NODE will be removed, so point TAIL to NULL)
		if (this.head === null || this.size === 1) { // Both Conditions does the same.
			this.tail = null;
		}; // Fix ends here; the below is the original code
    this.size--;
    return value;
  }

	// Linear Time Complexity O(n) -> because of the WHILE Loop.
  removeFromEnd() {
    if (this.isEmpty()) {
      return null;
    }
    const value = this.tail.value;
    if (this.size === 1) {
      this.head = null;
      this.tail = null;
    } else {
      let prev = this.head;
      while (prev.next !== this.tail) {
        prev = prev.next;
      }
      prev.next = null;
      this.tail = prev;
    }
    this.size--;
    return value;
  }

  reverse() {
    let current = this.head;
    let prev = null;
    let next = null;
    while (current) {
      next = current.next;
      current.next = prev;
      prev = current;
      current = next;
    }
    this.tail = this.head; // This is a fix for a bug that's already fixed 
    // in his repl.it; because it was pointed as a "fix" inside the YouTube comments.
    this.head = prev;
  }

  print() { // No changes in Linked List with a Tail IMPLEMENTATIONS
    if (this.isEmpty()) {
      console.log("List is empty");
    } else {
      let curr = this.head;
      let list = "";
      while (curr) {
        list += `${curr.value}->`;
        curr = curr.next;
      }
      console.log(list);
    }
  }
}

module.exports = LinkedList;

/** Uncomment when testing only this file */
/**
const list = new LinkedList();
list.append(1);
list.append(2);
list.append(3);
list.prepend(0);
list.print();
console.log(list.getSize());
list.removeFromFront();
list.print();
list.removeFromEnd();
list.print();
*/
```
##### Linked List Stack Data Structure ([source](https://www.youtube.com/watch?v=-0ZIresFUZI&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=60))
- 0:10 Starts with a mistake calling it simply a "Stack Data Structure" by me: I think that's wrong way of calling it as it'll cause confusion with the Stack Data Structure itself!
- **0:30 He mentions the fact that below I've already noticed & written notes about: the `linked-list-tail` must be `export`ed and then `import`ed inside `linked-list-stack` if working with repl.it OR *by me: copy paste the code into a single console(VSCode, etc.)*.**
- 0:40 Reminders: Stack Data Structure follows LIFO principle **last in first one** -> in other words: we **insert** from **one end** AND ALSO **remove** from that very same **end.**
- 0:55 In A Linked List We can treat `insert`ing a node at the **start** of the List as the `push` method in Arrays AND **removing** the **node** from the **start** as the `pop` method in Arrays Data Structure.
- 2:10 Hence why there's 3 main methods in Linked List Stack Data Structure:
- `push(value)`.
- `pop`.
- `peek`.
- As well as a few **helper methods**:
- `isEmpty`.
- `getSize`.
- `print`.
- *(Well here I stop writing notes, as I do understand the logic behind the **CODE flow**, but I don't understand the real life use cases, so I consider this to be enough of a notes.)*
### Linked List Stack IMPLEMENTATIONS:
# WARNING/NOTICE of a Possible BUGS:
- **I NOTICE THAT Linked List with a Tail has to be imported and included in the same file if I do use repl.it or CodeSandBox OR included in the same file by copy-paste `linked-list-tail` code inside the code `linked-list-stack` for the part below to work:**
```js
const LinkedList = require("./linked-list-tail");

class LinkedListStack {
  constructor() {
    this.list = new LinkedList();
  }

  push(value) {
    this.list.prepend(value);
  }

  pop() {
    return this.list.removeFromFront();
  }

  peek() {
    return this.list.head.value;
  }

  isEmpty() {
    return this.list.isEmpty();
  }

  getSize() {
    return this.list.getSize();
  }

  print() {
    return this.list.print();
  }
}

const stack = new LinkedListStack();
console.log(stack.isEmpty());
stack.push(20);
stack.push(10);
stack.push(30);
console.log(stack.getSize());
stack.print();
console.log(stack.pop());
stack.print();
console.log(stack.peek());
```
##### Linked List Queue Data Structure ([source](https://www.youtube.com/watch?v=15q-fLZqo_0&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=61))
- 0:20 Reminders: Queue Data Structure follows the FIFO principle: **first in first out** -> in other words we **insert from one end** and **remove from the other end.**
- In A Linked List we can treat `insert`ing a **node at the end of the List** as the `enqueue` operation AND **removing a node from the start of the List** as the `dequeue` methods operations.
- 0:50 Again, here we **import** the ~`linked-list`~ -> wait he has a mistake importing `linked-list` instead of as per his correct [replit](https://replit.com/@Codevolution/JavaScript-Data-Structures#linked-list-queue.js) importing the `linked-list-tail` Class -> _OR perhaps it doesn't matter for this Linked List Queue Data Structure whether the `-tail` part of the Linked List or the normal non-`tail` Class is imported both should work just as fine_ (*importing using `require`*).
- 1:30 3 Main Methods Operations in Linked List Queue DS:
	- `enqueue(value)`.
	- `dequeue()`.
	- `peek()`.
- And a few helper methods:
	- `isEmpty()`.
	- `getSize()`.
	- `print()`.
- 2:00 The `enqueue` method will **insert** a value at the rear end of a Queue.->For better understanding let's consider the `tail` of the List as the **rear end of the Queue.**-> To **insert a value at the `tail`** we can call `append` method on the List like so: `this.list.append(value)`.
### Linked List Queue IMPLEMENTATIONS:
# WARNING/NOTICE of a Possible BUGS:
- **I NOTICE THAT Linked List with a Tail has to be imported and included in the same file if I do use repl.it or CodeSandBox OR included in the same file by copy-paste `linked-list-tail` code inside the code `linked-list-queue` for the part below to work:**
```js
const LinkedList = require("./linked-list-tail");

class LinkedListQueue {
  constructor() {
    this.list = new LinkedList();
  }

  enqueue(value) {
    this.list.append(value);
  }

  dequeue() {
    return this.list.removeFromFront();
  }

  peek() {
    return this.list.head.value;
  }

  isEmpty() {
    return this.list.isEmpty();
  }

  getSize() {
    return this.list.getSize();
  }

  print() {
    return this.list.print();
  }
}

const queue = new LinkedListQueue();
console.log(queue.isEmpty());
queue.enqueue(10);
queue.enqueue(20);
queue.enqueue(30);
console.log(queue.getSize());
queue.print();
console.log(queue.dequeue());
queue.print();
console.log(queue.peek());
```
##### Doubly Linked List ([source](https://www.youtube.com/watch?v=MZhQB8R33xw&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=62))
- 0:10: What we have learned so far is Singly Linked List Data Structure -> contains nodes which have a `value` field as well as `next` field which points to the next node in the line of Nodes.
- 0:25 The other type is Doubly Linked List DS: each node contains the `value` field, the `next` **node link**, as well as a second link pointing to the `previous` node in the sequence -> this makes `insert`ion and `removal` in Constant Time Complexity **O(1)** but at the expense of more Space Complexity -> so, tradeoffs are Space Complexity increases.
- 0:55 [Visual explanation video by YouTube](https://youtu.be/MZhQB8R33xw?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=55) @codevolution.
- A node in doubly linked list ontains **2 pointers:** `previous` and `next`.
- With this in mind lets understand the **3 methods/operations**:
	- `insert`.
	- `delete`.
	- `traversal` operations.
- Quick timestamps though:
- 1:11 `prepend` method in Doubly Linked List:
	- **Case 1:** If the list is empty -> the new node is both the Head and Tail of the List. 
	- **Case 2** However if the List already exists -> we point `node.next` to `this.head` ; `this.head.previous` to `node`; and assign `this.head =` to the **new node which is the first node in the updated List**.
- 1:45 `append` method:
	- **Case 1** if the list is empty then the new node is both the Head and the Tail of of the List. However,
	- **Case 2** if a list already exists: `tail.next` will point at the **new node** and `node.previous` will point back at **Tail** ; finally we assign `tail` to the **new node** which is the **last node in the updated List**.
- 2:10 `removeFromFront` a node from the **front**: All you have to do is point the **head node** to its `next` **node.** -> The **first node** will effectively be removed in doing so (meaning: *technically removed/logically removed without a **direct** removal*).
- 2:28 `removeFromEnd` we have 2 scenarios:
	- **Case 1** if the list contains only 1 node->both head and tail of the List shoudl be pointed to `0`. This will effectively remove the node from the List (meaning automatically will be removing the node from the List without manually removing it) ->
	- 2:45 **Case 2** if the list contains **more than 1 node** -> you get hold of the node `previous` to the `tail` using `tail.previous` -> then Update the new `tail` pointer `next` field to null: meaning assigning `tail.next = null`
	- 3:00 Deletiong at both ends in both ends has Constant Time Complexity **O(1) in Doubly Linked List Data Structure**.
	- To `print` the list -> start at **head** and travel until the **tail** using `next` field on each node.
	- To `print` the list **in Reverse** -> start at **tail** and travel until the **head** using the `previous` field on each node -> it really is that simple.
- 3:40 Code Breakdown won't be provided, but Code itself is already in the replit / repl.it
- Doubly Linked List is much easier to be understood than Linked List.
### Double Linked List IMPLEMENTATIONS:
```js
class Node {
  constructor(value) {
    this.value = value;
    this.prev = null;
    this.next = null;
  }
}

class DoublyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  isEmpty() {
    return this.size === 0;
  }

  getSize() {
    return this.size;
  }

  prepend(value) {
    const node = new Node(value);
    if (this.isEmpty()) {
      this.head = node;
      this.tail = node;
    } else {
      node.next = this.head;
      this.head.prev = node;
      this.head = node;
    }
    this.size++;
  }

  append(value) {
    const node = new Node(value);
    if (this.isEmpty()) {
      this.head = node;
      this.tail = node;
    } else {
      this.tail.next = node;
      node.prev = this.tail;
      this.tail = node;
    }
    this.size++;
  }

  removeFromFront() {
    if (this.isEmpty()) {
      return null;
    }
    const value = this.head.value;
    this.head = this.head.next;
    this.size--;
    return value;
  }

  removeFromEnd() {
    if (this.isEmpty()) {
      return null;
    }
    const value = this.tail.value;
    if (this.size === 1) {
      this.head = null;
      this.tail = null;
    } else {
      this.tail = this.tail.prev;
      this.tail.next = null;
    }
    this.size--;
    return value;
  }

  print() {
    if (this.isEmpty()) {
      console.log("List is empty");
    } else {
      let curr = this.head;
      let list = "";
      while (curr) {
        list += `${curr.value}<->`;
        curr = curr.next;
      }
      console.log(list);
    }
  }

  printReverse() {
    if (this.isEmpty()) {
      console.log("List is empty");
    } else {
      let curr = this.tail;
      let list = "";
      while (curr) {
        list += `${curr.value}<->`;
        curr = curr.prev;
      }
      console.log(list);
    }
  }
}

const list = new DoublyLinkedList();
list.append(1);
list.append(2);
list.append(3);
list.prepend(0);
list.print();
list.printReverse();
list.removeFromEnd();
list.print();
list.removeFromFront();
list.print();
```
10. Hash Table Data Structure
- NOTES:
- There's Hash Table Collisions
##### Hash Table Overview ([source](https://www.youtube.com/watch?v=-Df9ipREbuM&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=63))
- A Hash Table also known as Hash Map (*or Hashtable*) is a Data Structure that is used to store key-value pairs.
- Given a Key, you can associate a value with that Key for very fast lookup.
- 0:30 You might ask "If we have JavaScript Objects why do we need Hash Tables?" - yes and the differences:
- JavaScript's Object is a special implementation of the **Hash Table** Data Structure. However, Object **Class** adds its own Keys. Keys that you input may conflict and overwrite the ihnerited default properties.
- Maps Objects which were introduces in ECMASCRIPT 2015 ES6 allows you to store key-value pairs -> 1:11 and to be honest that is what you should use when writing code.
- 1:20 Altough JavasCirpt already has two Hash Table implementations (Maps and Objects) -> writing your own Hash Table is a very popular JavaScirpt interview questions
- 1:50 Hash Table store key value pairs; Hash Table example:
	- `in` key => `India` value
	- `au` key => `Australia` value, etc.
- We store the key value pairs in a fixed size Array.
- Arrays have a number index. -> But a Stringified one.
- How to go from using **string** as an index to number as an index? -> The answer is the Hashing Function:
- A Hashing Function accepts the string key, converts it into a Hash Code using a defined logic and then map it into a numeric index that is within the bounds of the Array. -> Once we have the index:
- Using the index we store the `value`.
- The same Hashing Function is reused to `retrieve` the value given a key.
- A hash table supports 3 main methods / operations:
	- `set` to store a key value pair
	- `get` to retrieve a value given its key
	- `remove` to delete a key value pair
- [3:15](https://youtu.be/-Df9ipREbuM?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=214) timestamps Hash Table Visualization.
- 3:33 Hash Table Usge:
	- Hash Tables are tpyically implemented where Constant time lookup and insertion are required such examples are:
		- Database indexing;
		- Caches.
##### Hash Table Implementation ([source](https://www.youtube.com/watch?v=y3CcHKEM8r8&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=64)]
- Hash Table breakdown logic:
	- 3 Main operations/methods `set`, `get` and `remove`.
	- Additionally we need to implement Hasing Function to convert a **string** key to a **numeric index** (*index of **number** data type!*).
- [**`hash` method**](https://youtu.be/y3CcHKEM8r8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=111) accepts `key` argument which is of type **string** data type.
- 2:00 The logic to convert `key` into number index can vary in complexity -> we want complex hashing functions that **do not produce** the same indexes for different Keys. (*by me: we should **never** have duplicate indexes clashing*. Oh, **UPDATE: that's called collisions definitions: [at 0m in the next video about Hash Table Collissions](https://www.youtube.com/watch?v=kTh5nAqCkOA&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=65): if `hash` method produces the same indexes for different Keys resulting in a loss of data!** Aha-moments! -> Also as an example he [showed](https://youtu.be/y3CcHKEM8r8?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=530) using 2 different Keys (`name` and `mane`) but same `charCodeAt` AND it still overwritten the **first** Key Value pair of a key `'name'`.) -> However for beginners perspective we'll use a very simply logic `hash` method -> using the `charCodeAt` built-in method to **convert** **each character** in the Key into a **numeric value**. -> Then we will **add** all the values to give us one **number** which we can use.
- 3:15 To make sure the index is in bounds/borders/limits of the Array's total amount of items -> we'll use the modulus operator against the `size` of the Array; code-wise: `return total % this.size;` inside `hash` method.
- `set`;`get`.
- 5:50 `remove(key)` method You can **add** an `if` condition inside `remove` method to **check** if **Value** exists **before** deleting; but for our implementations this works just as fine.
- UPDATE FROM ME OK, BUT he has mistakes in his supposed to be fixes "Hash Table Collisions Video.
- -> his previous code was `this.table[index] = undefined;` well, if that was wrong; now he has it even worse in the next video at [5:50](https://youtu.be/kTh5nAqCkOA?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=350) he has unnecessarily 3 methods calls and the **good** part is he *at least* avoided Cubic Time Complexity O(n^3) -> but the **worst** part is that he still calls `indexOf` method as the argument to the `splice` method which seems to be Linear Time Complexity O(n^2). Below in my next video-setion-notes I will try to implement my own fixes.
- `display` method by me: seems to be of Linear Time Complexity O(n) because it uses a `for` loop.
#### Hash Table Collisions ([source](https://www.youtube.com/watch?v=kTh5nAqCkOA&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=65)).
- 1:20 The changes required for the fix are modifying `set` and `remove` methods as well **as the main `get` method** (so, `get` method was the **main issue**).
- So here's the **previously wrong code** (*which do not exist in the final code I have below is the **fixed HashTable***):
- **Previously wrong** `set` method (which in the final code gets much more wordy):
```js
set(key, value) {
  const index = this.hash(key);
  this.table[index] = value;
};
```
- **Previously wrong** `get` method (which in the final code gets much more wordy):
```js
get(key) {
  const index = this.hash(key);
  return this.table[index];
};
```
- **Previously wrong** `remove` method (which in the final code remains simple, just with _modified logic_):
```js
remove(key) {
  const index = this.hash(key);
  this.table[index] = undefined;
}
```
- **5:20** His previous code was `this.table[index] = undefined;` well, if that was wrong; now he has it even worse in this same video at [5:50](https://youtu.be/kTh5nAqCkOA?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=350) he has unnecessarily 3 methods calls and the **good** part is he *at least* avoided Cubic Time Complexity O(n^3) -> but the **worst** part is that he still calls `indexOf` method as the argument to the `splice` method which seems to be Quadratic Time Complexity O(n^2). Below I will try to implement my own fixes:
#### FIX #1 using `findIndex` to avoid calling `indexOf` as an argument to the `splice` method (*again, that's inside `remove` method*):
- Also I've renamed `sameKeyItem` into `sameItemIndex` for a better naming convention.
```js
// ...
if (bucket) {
	const sameItemIndex = bucket.findIndex((item) => item[0] === key);
	if (sameItemIndex !== -1) {
		bucket.splice(sameItemIndex, 1);
	};
};
```
#### Fix #2 **only using `filter`** method meaning: omitting `splice`:
- Explanations: because `filter` method `return`s an empty Array literal `[]` if no items matches the condition, its result is safe to be assigned to `bucket`.
	- Again, I could have used an `if` condition to manually assign `bucket = []` but that's unnecessarily redundant.
	- Explanation further: Viewing my code AND comparing it to the original code creates confusion as if where's the **deleted item** which `splice` method with 2nd argument of `1` removes that item at a `indexOf` the `sameKeyItem` inside the `bucket` Array (original code: `bucket.splice(bucket.indexOf(sameKeyItem), 1)`) -> and instead in my code "filters out the Array" which Array is the key value pairs, again reminders: Key is at `0` index and Value at `1` index **-** that's a maximum of 2 items strict rule.
- **A worthy note** is that previously I had a bug: `bucket = filteredBucket` is **wrong** because in this way the **code** will **only** update the **value** of the `bucket` variable **within the function scope**. It **won't** update the actual `this.table[index]` **value**.
	- That's a very tricky bugs (keywods: new aha-moments; tricky parts of JavaScript I never knew existed; I'd have totally failed answering such a question have I been asked about it correctly - instead I'd have given a wrong answer saying they are the same thinking tis the `this.table[index]` pointer that's being updated by using `bucket` variable but, turns out it's **not**).
```js
// ...
if (bucket) {
	const filteredBucket = bucket.filter((item) => item[0] !== key);
	this.table[index] = filteredBucket;
};
```
- **7.50:** increasing teh SIZE oF the Array iS **NOT** the BEST Solution to handling collisions-> Sure it may **reduce** the number of collisions **but** the **possibility** of **data loss remains!**
- Typically whenever the Hash Table reaches half the capacity or more -> the Array capacity is doubled and the Key Value pairs are rehashed.
- 8:20 Time Complexities:
  - Having Array method `find` being called in all 3 of the `set`, `get` and `remove` methods we have a Linear Time Complexity
  - However, with Hash Tables the collision is very minimal and it can be reduced to a great extent by having a better `hash`ing functions -> because of that we generally consider the **Average Case Time Complexity** instead of **Worst Case Time Complexity** when it comes to **Hash Tables** -> the **Average Time Case Complexity** is **Constant Time Complexity O(1)** -> that is the reason Hash Tables are a **prime** **choice** when solving problems.
### Final Hash Table Implementations code-wise (*with the collisions fixed by the modified `get` and `remove` methods.*):
```js
class HashTable {
  constructor(size) {
    this.table = new Array(size);
    this.size = size;
  }

  hash(key) {
    let total = 0;
    for (let i = 0; i < key.length; i++) {
      total += key.charCodeAt(i);
    };
    return total % this.size;
  };

  set(key, value) {
    const index = this.hash(key);
    const bucket = this.table[index];
    if (!bucket) {
      this.table[index] = [[key, value]];
    } else {
      const sameKeyItem = bucket.find((item) => item[0] === key);
      if (sameKeyItem) {
        sameKeyItem[1] = value;
      } else {
        bucket.push([key, value]);
      }
    }
  }

  get(key) {
    const index = this.hash(key);
    const bucket = this.table[index];
    if (bucket) {
      const sameKeyItem = bucket.find((item) => item[0] === key);
      if (sameKeyItem) {
        return sameKeyItem[1];
      }
    }
    return undefined;
  }

  remove(key) {
    let index = this.hash(key);
    const bucket = this.table[index];
    if (bucket) {
    // // Below code is O(n^2) Linear Time Quadratic Time Complexity:
    // // Calling `indexOf` as an argument to `splice` - that's wrong; as it can be better.
    // const sameKeyItem = bucket.find((item) => item[0] === key);
    // if (sameKeyItem) {
        // bucket.splice(bucket.indexOf(sameKeyItem), 1);
    // }
	
    // Instead a (#2) fix would be of a Linear Time Complexity:
        const filteredBucket = bucket.filter((item) => item[0] !== key);
        this.table[index] = filteredBucket;
    };
};


  display() {
    for (let i = 0; i < this.table.length; i++) {
      if (this.table[i]) {
        console.log(i, this.table[i]);
      }
    }
  }
}

const table = new HashTable(10);
table.set("name", "Bruce");
table.set("age", 25); // NOTE to avoid confusion: That's a key-value pair irrelative of the `name: Bruce`.
console.log('-- Display 1:');
table.display(); // Chrome's DevTools would rather print the FINAL HashTable that's below at the bottom.
console.log(table.get("name"));
table.set("mane", "Clark");
table.set("name", "Diana");
console.log(table.get("mane"));
table.remove("name");
console.log('-- Display 2:');
table.display();
```
11. Tree Data Structure AKA _almsot always referred to as_ Binary Search Tree Data Structure
##### Helpful video by [@codevolution](https://www.youtube.com/watch?v=c-LEpmYikFY&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=66) again.
- A Tree is a hierarchical data structure that consists of nodes connected by edges
- A **Tree** is a **non-linear** Data Structure, compared to Arrays, Linked Lists, STacks and Queues which are all Linear Data Structures. -> That is very important because:
- In Linear Data Structures, the time required to Search is proportional to the **Size** of the Data Set.
- Trees however, owing to the nonlinear nature allow quicker and easier access to the Data.
- A tree will **not** contain any loops or cycles.
- 1:00 Visualization: each **Nodes** are **connected** by **Edges.**
- Tree Data Structure usages:
  - File Systems for directory structure
  - A family tree
  - An organisation tree
  - More relatable examples:
    - DOM
    - Chat bots
    - Abstract syntax trees, etc.
- Trees are often asked about during interviews (by me: well everyone can ask everything from DSA, nobody can tell you its real world usage because it doesn't exist, *unless you are inventing a new Database from scratch*; so I don't care about which, but I care about memorizing the most of the basics so I can branch off of them.)
- Tree terminology (/Binary Search Tree Data Structure Terms):
  - Nodes
  - Edges
    - Parent node - a node that is an immediate predecessor of any node.
    - Child node - a node that is an immediate successor of any node.
    - Root node - the node from which the *Tree* originates. -> It does **not** have a parent node.
    - Leaf nodes - nodes that do **not** have any child nodes (_the last nodes; at the bottom of a Tree_).
    - Siblings nodes - nodes with the same parent.
    - Ancestor nodes - such as parent's parent (_something like a grandparent_).
  - More Terms:
    - Path - the **sequence** of **nodes** and **edges** from **one node** to **another**. -> Something like Ancestor nodes to its GrandChildren node or in reverse - the Path from one end to the other end is the Path definition.
    - Distance - along similar lines we have **distance** which is the **number** of **edges** along the **shortest path** between **2** nodes (*by me: is this edge cases I'm not sure*).
  - **NODE Properties** (he perhaps means TREE Properties?):
Visual example:
```
       A
     / | \
    B  C  D
   /\
  E  F
```
- **NODE PROPERTIES explanations:**
	- **The **Degree** of a Node** is a **total number** of **child nodes** it **has**.
		- Example: Degree of a Tree is the **maximum** Degree of a Node in the Tree.
		- Degree of `B` is `2`.
	- **The **Degree** of a Tree** is the **maximum degree of a node in a tree.**
	- **The **Degree** of a Tree VC** (What's VC? _[3:50](https://youtu.be/c-LEpmYikFY?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=230) visual explanations_).
		- Google 0 Results for "*What is VC of a Tree in programming*"; except for one closely related to - a [stackexchange question](https://stats.stackexchange.com/questions/231089/what-is-the-vc-dimension-of-a-decision-tree) -> which has a hyperlink to wikipedia [Vapnik–Chervonenkis dimension](https://en.wikipedia.org/wiki/Vapnik%E2%80%93Chervonenkis_dimension).
		- Anyways I don't understand what that is but,
		- Example: the degree of the Tree VC is `3` which is the **degree of node `1`.** -> which is the **degree of node `A`** -> Not sure why he says that without any explanations? My best guess would be that it's the **root node's children numbers** - like their total closest children, but not their grandchildren nor total nodes in the Tree, I guess.
	- **The **Depth** of a Node** is the **number of Edges** from the Root up to that Node. -> It's like the Path starting from `0` / TOP Node / Node `A` in this example, up to that given Node: so, the **"amount of Nodes connecting up until that given Node"** is the **Depth of a Node**, I guess.
		- Example: the **depth** of **root node** is **always `0`.**
		- Depth of node `E` is `2`.
	- The **Height** of a Node is the **number of Edges from the deepest leaf up to that Node.**
		-  Example: the **height** of the **root node** is considered the **height** of the **whole Tree**, whichever that amount is, in this case `2`.
		-  Height of node `B` is `1`.
		-  So, I guess by that definition that the height of `E` and `F` and **even** `C` and `D` is `0` for all of them.
	-  (*These terms needs to be memorized after they are understood, there's no way around it.*)
- For Tree interview questions you are rarely asked to implement a generic **Tree**, but rather a specific type of **Tree** which is the **Binary Search Tree.**
- (*That's frustrating learning DSA to pass an interview is cheating; rather than learning anything that will be actually useful and show my problem solving skills, I have to memorize solutions to a problem which anyone else who does that as well doesn't showcase a problem solver employee but they're being good at a memory game in order to bypass the interviews for the most of the companies out there (even big and small sized ones).*) That's sad.
- Characteristics of Binary Search Tree Data Structure are described using those Terms and Properties. -> So, they're important to be **memorized** in **order** for me to **understand** Binary Search Tree. (*Okay, pause here, this needs a bit of **repetition** before I proceed, so that I'll struggle less with BST.*)
12. Binary Search Tree Data Structure (BST DS)
- A Binary Tree is a Tree Data Structure in which each node has at most **2 children** (maximum 2 children nodes).
- They are referred to as **left child** and **right child.**
- Binary Search Tree has the following 2 properties:
	- The value of each **left node** **must** be **smaller** than the parent node.
	- The value of each **right node** **must** be **greater** than the parent node.
	- This is all in addition, each node has at most 2 children nodes.
	- (_So they are branching off of it, like each node can hold 2 more nodes and so on._)
- Visual representation where each node has a maximum of **2 child nodes:**
```
    10
    / \
   5  15
  /\
 3  7
```
- Binary Search Tree operations:
- `insert`ion - to **add** a node to the tree.
- `search` - to **find** a node given its value.
- DFS & BFS - to **visit** all nodes in the tree.
  - Explanations: Depth First Search & Breadth First Search -> ok these are terms I don't fully understand. Needs review/revisit on my side.
- `delete` - deletion - to **remove** a node given its value.
- Binary Search Tree usages:
  - Searching
  - Sorting
  - To implement abstract Data Types such as lookup tables and priority queues.
    - Spanish village, needs review/revisit.
    - He suggests googling these terms and understanding the concepts (_so @codevolution won't explain it in every details_).
    - "This is a very high level use case for BST, but I want you to make a note to google about these contexts, once we're done implementing it".
    - "When you learn how **performant** BST are with th different operations you will realize why it is a good choice" -> so BST are great choice.
#### Binary Search Tree Class ([source](https://www.youtube.com/watch?v=kIVkBsfB-SM&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=68))
- -> In this video is implemented the Node Class and the Binary Search Tree Class with the `consturctor` and `isEmpty` methods.
- In the _next video_ `search`, `traversal` and `delete` operations will be explained.
- Each Node contains a Data Value and optional pointers to the left and right child nodes.
- In an isolated Node it contaisn a Data Value and the left and right pointers pointing at `null`.
- (I may or may not write whole lengthy notes in here, but final code I'll paste from [replit](https://replit.com/@Codevolution/JavaScript-Data-Structures#binary-search-tree.js) by @codevolution YouTube series in Video's descriptions.)
- 2:05 A new Node will not have any child nodes to begin with (*so perhaps the subsequent nodes will do have?* -> YEP he then says we'll add a new Node to the Tree in the next video).
- Back at the Visual Slide with a Tree with 5 Nodes:
- To work with Binary Search Tree we always make a Pointer to the Root Node in the Tree. That Poitner is cruical to almsot every operations we perform on a BST. However, when the a Tree is empty, there's no Root to point to that, hence the Root Pointer will point at `null`. (so that's the logic behind BST.)
  - Code wise: `this.root = null` (of inside `consturctor` inside `BinarySearchTree`). 
#### Binary Search Tree Insert ([source](https://www.youtube.com/watch?v=zOgsIjM-a7g&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=69))
- 0:20 Recursion is very important when it comes to BST -> so Recursive functions simplifies BST code.
- 0:25 Empathy: you may not understand the code right away, you might need to spend a time tracing the function execution of each method with a sample Tree using a pen and a paper => by me: I rather take digital notes.
- 1:50 2 Scenarios real life cases.
- **I'll skip the next notes and keep this in mind and try to go to LeetCode. Because I lose motivation trying to memorize them but not seeing any real life application of them - not that any exists, but at least to be 'humanized' as is the case with LeetCodes. **
- (*Of course, that is, after I'm done with copying the Graph Data Structure **below** as well with a very minumum notes; so that way I'll finish the @codevolution free series on YouTube playlist and come back as per needed once I struggle with LeetCodes & learn something new OR rewatch his implementations.*)
---
- _Coming back with new notes, I'm rewatching and understanding it all, however I still don't see where would I apply any of this knowledge, I only appreciate the fact that I can be more aware of the Space Complexity and Time Compexity of my code, but anything beyond it like trying to build Data Structures that I, if ever, would have to use is kinda pointless, since I will still Google or I will still come back to my notes or plainly using Sets or Maps as per needed._
  - _Oh and I also learned that usually improving Time Complexity comes at a cost of Space Complexity and Vice Versa._
#### Binary Search Tree Search method is too easy ([source](https://www.youtube.com/watch?v=lml2E9SIJHo&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=70&pp=iAQB)). 
- Checking if `root.value === value` then `return true`, otherwise we check if `value` is less or bigger (/greater) than the `root.value` then we accordingly call `search` method recursively like so:
  - `if (value < root.value)` then we `return this.search(root.left, value);` (because `value` is smaller than `root.value` then we traverse the **left subtree**) -> otherwise `else` (when `value` is greater than `root.value` we traverse the **right subtree*): `return this.search(root.right, value);` -> until base cases returns a `Boolean` at either the first condition `if (!root) return false;` or the second base case condition `if (root.value === value) return true;`.
#### Binary Search Trees Depth First Search - BST DFS ([source](https://www.youtube.com/watch?v=n6_Ruq1qvjU&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=71)):
- DFS Algorithm starts at **root node** and explores as far as possible along each branching before backtracking (*by me: we visit every possible **leafs** terms as they are usually called).
- We need to visit the root node, visit all the nodes in the **left subtree** and visit all the node sin the *8right subtree**.
- Visual representation of a BST example for better understanding:
```
    10
    /\
   5 15
  /\
 3  7
`````
There are 3 types of DSA traversals:
  1. Preorder algorithm:
     1. Read the data of the node.
      2. Visit the left subtree.
      3. Visit the right subtree.
     4. -> So, the preorder traversal for this BST is: 10, 5, 3, 7, 15.
  2. Inorder Algorithm:
      1. Visit the left subtree.
      2. Read the data of the node.
      3. Visit the right subtree.
      4. -> So, the inorder traversal for this BST is: 3, 5, 7, 10, 15 (*Visiting Root Node 10, then 5 (left) then 3 (left)* -> since no more subtrees on the left, then 2nd Step: Read the data-> **3** has no right subtree either, so go back to the parent **5** -> **5**'s left subtree has been **visited** so step 2: Read the node **5** so step 3: visit the right subtree of **5** -> node **7** has no left subtree so read its value, so node **7** does not have any right subtree so go back to the parent 5 but which has been read already -> so 	go back to the parent which is now the Root Node (meaning: whole left subtree of the Root Node has been visited) -> now we read the Root Node value **10** -> next we visit the right subtree -> there's only one node **15** and no further left subtrees to visit so Read the node value of **15** -> there's no right subtree to visit either -> at this stage all the nodes has been visited -> and 3, 5, 7, 10, 15 is the Inorder Traversal for our BST.).
  3. Postorder Algorithm:
      1. Visit the left subtree.
      2. Visit the right subtree.
      3. Read the data of the node.
      4. -> So, postorder traversal for this BST: 3, 7, 5, 15, 10. -> Here's my own breakdown: Starts at Root Node -> checks the deepest leaf node (hence the term **Depth Search** - the node at the bottom) on the left subtree: 3 -> checks for node 3's right subtree: none -> so goes up to the parent 5: does it have a right subtree node -> it does: 5 -> then goes up to 7 -> 7 has been visited and already read -> so 'push' (incorrect term I use for better understanding) 7 as well -> again at the Root Node 10 -> does it have right subtree -> it does have 15 -> 15 is 'pushed' -> 15 does not have right subtree -> we go back at Root Node 10 -> 10 is finally pushed as the last Node whose all its children AKA both subtrees has been read and visited already.
#### Breadth First Search BFS ([source](https://www.youtube.com/watch?v=H0i3gk1h0lI&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=72)).
- BFS explores all nodes at the present **depth** prior to moving on to the nodes at the **next depth level.**
- For example with visual representations BFS for easier understanding:
```
    10
    /\
   5 15
  /\
 3  7
`````
- BFS Traversal example: BST with 5 nodes: 10, 5, 15, 3, 7. Explanations: 1. BFS visits the Root Node first: 10 -> 2. Move onto the Nodes on the next level (below it; left subtree for example): 5 -> 3. Then visits the right subtree: **15** -> 4. And the levels afterwards, so from Parent Node 5 left subtree onto the: **3** -> 5. Then right subtree of 5: node **7.** As the name indicates: traverse the **Breadth First.**
- BFS Traversal Approach:
  1. Step 1: Create a queue.
  2. Step 2: Enqueue the **root node.**
  3. Step 3: As long as a node exists in teh queue, perform the following operations:
      1. Dequeue the node from the front.
      2. Read the node's value.
      3. Enqueue the node's left child if it exists.
      4. Enqueue the node's right child if it exists.
- Operations forthis is inside the `levelOrder` method.
#### Binary Search Tree Min Max ([source](https://www.youtube.com/watch?v=mrzE5SqzoQY&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=73)).
- Finding Min and Max is a prerequisite for the next section: Delete method.
- It's very easy, since the most left leaf node is the smallest value in the tree, and the rightmost leaf node is the largest value in the tree. -> So, if it's not the first `root.left` or `root.right` value respectively for whoever we are searching for -> then, we keep calling the function recursively.
#### Binary Search Tree Delete - BST Delete ([source](https://www.youtube.com/watch?v=80GhW9X1MGI&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=74)).
- Visual examples:
```
    10
    /\
   5 15
  /\
 3  
`````
- There's 3 scenarios to `delete` method:
  - Scenario 1: the **node** to be deleted is a **leaf node** which means: it has no **child nodes** -> for example Node **3.** 
  - Scenario 2: the **node** to be deleted has one child node -> lets say **5** -> in this case remove the node **5** and **replace** it with its child node **3** -> it's applicable for both left or child node.
  - Scenario 3: the **node** to be deleted has three children nodes -> in this example say it's **10** AKA the **Root Node** -> in this case we **COPY** the value of the **Inorder successor** to the node **&** **DELETE** the very same **Inorder successor** -> AND -> the **Inorder successor** of a Node in a Binary Search Tree is the **next node** in the **Inorder traversal sequence** -> AND -> in a Binary Search Tree the **Inorder successor** is the node with the **least value** in its right subtree (so by me I guess that's ~*the last leaf in the right subtree AKA the rightmost node?*~-> and I was wrong! Since rightmost leaf node is the **highest** value, but my confusion remains (read below)). -> In our example the right subtree for the **root node 10** has just one node **15** -> which automatically becomes the minimum value -> so replace the Root Node's value with the minimum Node's value which is **15** & **remove** that corresponding *node 15***.**
   - By me I'm still confused: that is too simple of an example for the **most complex step 3!** What if node **15** had 2 children nodes? What happens to the right subtree? Does it vanishes forever? -> Because, if **15** had 2 children: 14 on the left, 16 on the right, who would replace 15? Since "both will" is impossible -> because we already have left subtree with parent node **5** and its child node **3**, so we can't overwrite them either? I guess that's an edge case he has yet to talk about (@codevolution)?
- Next, I don't understand his `if (root === null) return root;` is more important he says at 4:04m into the video, as if there's multiple nodes so that recursion will keep working, huh? Whereas he says if it `return null` then, then he actually says nothing but indirectly suggests as if that's not gonna work out?
- Code breakdown:
- Condition `if (value < root.value)` -> then we traverse the left subtree: `root.left = this.deleteNode(root.left, value)`. -> `else if (value > root.value)` then we traverse the right subtree `root.right = this.deleteNode(root.right, value)` -> `else` if both these conditions failed then we have found the **node** whose value is equal to the passed in `value` argument -> and in here it is where we need to address the 3 Scenarios from the visual breakdown (above):
  - Scenario 1: the *node to be deleted* is a **leaf node** that means it has no child nodes: `if (!root.left && !root.right)` then `return null;`
  - Scenario 2: the *node to be deleted* has **one child node** -> in this case remove the **node** and replace it with its **child node:** `if (!root.left) {return root.right} else if (!root.right) {return root.left}`.
  - Scenario 3: If both scenarios checks above have failed then the *node to be deleted* has **two children nodes** -> in this scenario we **COPY** the value of the **Inorder successor* to the *node-to-be-deleted* and then **DELETE** the **Inorder successor.** -> The **Inorder successor** of a node is the **minimum** value in the `right` subtree. -> So, update the **root node**'s `value` property with the node that contains the **minimum `value`** in the right subtree: `root.value = this.min(root.right)` -> then we **delete** that node: `root.right = this.deleteNode(root.right, root.value)` -> finally to make sure the changes to the **root node** are reflected we `return root`.
  - **HE STILL DID NOT MENTION IF RIGHT SUBTREE HAS CHILDREN NODES!**
  - So I had to test it all by myself:
```js
bst.insert(10);
bst.insert(5)
bst.insert(15);
bst.insert(3);
bst.insert(14);
bst.insert(16);
bst.levelOrder(); // logs: 10, 5, 15, 3, 14, 16
bst.delete(10); //delete root node
bst.levelOrder(); // logs: 14, 5, 15, 3, 16
// I did not expect this. I guess recursive calls are happening.
```
- I did not expect this. I guess recursive calls are happening.
- I guess the calls to `min` method is happening because we must find the **lowest** node value in the **right subtree**, and once we found it and re-assigned it to the `root.value` property, then on the line of `root.right = this.deleteNode(root.right, root.value)` -> which `root.right` acts as the `root` argument#1 of the `deleteNode` method **&&**  `root.value` which now represents the **most minimum node value of the right subtree** is passed as the `value` argument#2 of the `deleteNode` method when called recursively.
- So, in my code trial above, I guess that order in human words is: 1. Found node **14** as the `min`imum node value on the **right subtree** -> 2. (Recursively calls/) Re-calls `deleteNode` method with now `root.right=15` **&&** `root.value=14`
  - Even ChatGPT confirms my logic (*before I even asked about it*): "this ensures that the binary search tree property is maintained because the **new value** is **greater** than **all** the **values** in the **left subtree** and **smaller** than **all** the **values** in the **right subtree.".**
  - `root.right = this.deleteNode(root.right, root.value);`: "this line recursively calls the deleteNode method on the right subtree with the updated value."(*ChatGPT*) -> OH, so wait, that means the call in my case self-made example could be as: `root.right = this.deleteNode(root.right=null, root.value=14);` -> so because `root.value=14` AKA the most minimum value **14** / right subtree's leftmost node value **14** is assigned to the `root.value` -> then does the `root.right` of the node **14** has a **right** node? - Nope, so that's why `root.right` is now `=` to `null`. Then let me write the full visual representation of my code self-made example above (***prior to removing the root node `10`***):
```js
    10
    /\
   5  15
  /   /\
 3   14 16
`````
- Oh and ChatGPT continue about the `root.right = this.deleteNode(root.right, root.value);`: "..This recursive call will delete the node with the minimum value from the right subtree. By doing this, we ensure that the node with the `min`imum value is effectively **removed** from the tree." -> by me: I think removed & "copied" AKA returned as if the **new `root` node?** Because, exiting all these conditions seems to `return root;` -> which again confusion clarification: it is because `delete` method has called this `deleteNode` method and that `delete` method needs to re-assign the root node at its line of: `this.root = this.deleteNode(this.root, value);`. -> My last confusion is how does "`return root;`: this line returns the **modified** **root** **node** after performing the deletion operation." - while I do understand this ***already*** -> rather the part I'm confused about is inside the `deleteNode` method: `if (root === null) return root;` why does this returns? I know it's a base case so it's stops re-executing the recursive calls, but how does the last line ***outside all of these conditions*** returns `return root;` as the "*modified root*" huh?
  - I asked ChatGPT about it and his answer is: "*You are correct in pointing out that the base case `if (root === null)` seems redundant in its current form.*". && I asked him "are you sure?" then he apologizes: "it is possible to reach a point where the `value` you want to **delete** is not found in the tree" -> and so I tested this command call: `bst.delete(11);` -> and indeed I got an error: "`Type Error: Cannot read properties of null (reading 'value')`"; but since my root node was **10**, then the recursive calls to `deleteNode` itself still happened:
    - Code tracing breakdowns:
    - I think I can break down the code like so #1: condition `else if (value=11 > root.value=10)` had ran the code: `root.right = this.deleteNode(root.right=15, value=11)` -> next #2: condition `if (value=11 < root.value=15)` then run: `root.left = this.deleteNode(root.left=14, value=11)` -> #3: condition: `if (value=11 < root.value=14)` then run: `root.left = this.deleteNode(root.left=null, value=11)` -> #4 Hence the limit is reached, there's no more nodes below to compare, so now the condition `if (root(null) === null)` runs and then returns `null` with `return root(null);`.
    - Nicey, I think I'm correct, let's trace it for removing node **10** then:
    - Trial#1: Step #1: None of the `if` and `else if` conditions aren't true, so we run inside the main `else` code block: none of these conditions apply either, so we go down at the code line of `root.value = this.min(root.right);` which assigns `root.value = 14` (*I guess now that we have a copy of it, we have to remove it & replace it as the new **root node***) -> #2: `root.right = this.deleteNode(root.right=15, root.value=14);` -> #2.1: `deleteNode(root=15, value=14)` is being run so, the condition: `if (value=14 < root.value=15)` runs the code: `this.left = this.deleteNode(root.left=14, value=14)` ***(wait so I realized that node `10` is gone)*** -> #2.2 `deleteNode(root=14, value =14)` runs, (*wait so now they are equal `root===value`*) -> so none of the topmost conditions apply then we enter the final `else` block: .. I see I'm tracing something wrongly, as I will reach the line of `root` being `null` again, until then, I don't know what did I missed but I'm just unable to reach the final #3 `return root;` at the bottom of inside `deleteNode` method? .. I would try and say #3: inside the `else` the condition `if (!root.left && !root.right)` is true and `return null;` which exits the recursive calls to finally `return root;`?
    - Failed#1: It seem like `return root;` always runs ~and it assigns~ or rather it just returns the modified `root` as its `root.value=14` (`min`imum of the right subtree): so that's the step #3 when it exits, but I need to trace how `root.right` gets re-assigned since that should have a value of **15** because the `root(14).right(15)` should be **15** (in my case example, otherwise they don't need to differentiate only by a value of **`1`**, I just chosen it like that myself quickly).
    - Failed#2: Let's go back at #2.1: `root.right(right node of 10 is 15) = root.deleteNode(root.right=15, root.value=14)` -> now I think by having it to return `null` means it removed the "copied" new-to-be-node & later makes it the new root wit hthe `return root;` -> but until now, continue: -> #2.2: **deleteNode** method receives these arguments: `deleteNode(root..` .. or rather it is an Object that is returned like so
    - Trial#3: Step #2.1: `root.right({value: 15, left: 14, right: 16}) = this.deleteMethod(root.right({value: 15, left: 14, right: 16}), root.value=14)` -> #2.2: `deleteNode(root({value: 15, left: 14, right: 16}), value=14)` then the condition that is truthy is: `if (value=14 < root.value({value: 15,left:14, right:16}))` which makes it run: `root.left({value: 14, left: null, right: null}) = this.deleteNode(root.left({value: 14, left: null, right: null}), value=14)` then -> (*I realize this all could be wrong; rather that `return root;` had run and assigned `root.right` its returned value?*) #2.3: `deleteNode(root({value: 14, left: null, right: null}), value=14)` runs, and none of the topmost conditions are truthy (*again*) -> so we enter the `else` block the condition `if (!root.left && !root.right)` is true and `return null;` runs (*again!*) which exits the recursive call *indeed*, but however I shouldn't confuse myself about trying to re-assign the `root.right`: because that does **not** matter & **actually** it **rather** **must** **remain** the **same** because I *only* need to remove the node **14** & place it as the new **root node**.
      - The assigning part is easy: `return root;` inside of `deleteNode` -> actually means `this.root = this.deleteNode(root, value)` that node **14** has been assigned as the **`this.root` node**. && At this point I'm left confused as to ***How is the node **14** removed in all of this code where `return null;` only happened when trying to re-assign `root.right` property?***
  - Back to asking ChatGPT answers: "*A node is considered deleted when one of the following conditions is met: **a.** If the node to be deleted is a leaf node (has no left or right children), it is removed from the BST by returning `null`.*" -> ok so that condition I already reached hence why it was the leaf node **14** of the right subtree's leftmost leaf node, -> so that's how node **14** was deleted & then returned by `return root;` cause it to be re-assigned as the new `this.root` line of code inside `delete` method.
  - Okay ChatGPT confirmations: "*6. The `min`imum `value` 14 **replaces** the `value` of the node 10, **effectively "deleting"** the **node 10** by **replacing it** with the `min`imum `value`.*" -> so **technically** it **removes** node **10** by replacing it with the value of the `min`imum value at the root's `right` subtree of the leftmost leaf node found at the step where the code `root.value = this.min(root.right)` executes! -> ChatGPT: "7. The code then recursively calls deleteNode on the right subtree to delete the minimum value (`14`) from the right subtree." -> "8. The recursive call on the right subtree continues until the base case is reached. The base case is when the current `root` node becomes `null`." -> "9. When the base case is reached in the **right subtree**, the current `root` node becomes `null`, and the recursive calls stop." -> "10. The modified `root` node (after the deletion process) is returned from the `deleteNode` function and assigned to `this.root` in the `delete` method." -> YET I'm still confused how was the leaf node **14** deleted? -> Then ChatGPT modifies his answers:
  - ChatGPT starting only from the modified step: "8. Now, the `deleteNode` method is called with the root node of the right subtree (`15`) and the value `14`." -> "9. Since `14` is **less** than the root value `15`, the code continues to the **left subtree** of the **right subtree**." -> "10. The `deleteNode` method is called again with the left child of the right subtree (`14`) and the value `14`." -> ~"11. The base case is reached because the current `root` node (`14`) is `null`. In this case, the recursive calls stop, and `null` is returned."~: nope, he's wrong; base case is reached because **leaf node 14** doesn't have any left or right children! -> I hit him up with that and he modifies his answer's step:"11. The base case is reached because the current **`root` node** (`14`) is a **leaf node** with no **left** or **right child**. In this case, the recursive calls stop, and `null` is returned." -> "12. The `deleteNode` method in the previous call assigns the returned value (`null`) to `root.right`, effectively deleting the node with the value `14` from the **right subtree** of the **`root` node (15)."** -> "13. The modified root node of the **right subtree** (`15`) is returned to the previous call.": **I have no idea how is this done if previous call returned `null`?** -> "14. The `deleteNode` method in the initial call assigns the returned value (`15`) to `root.right`, updating the right child of the root node (`10`).": **WHAT?** Did he mean the new **root node 14** and **not** *10*?
- And finally I guess visually after deleting node **10** - it would like this:
```js
    14
    /\
   5 15
  /    \
 3     16
`````
### Binary Search Tree code implementations (by [replit](https://replit.com/@Codevolution/JavaScript-Data-Structures#binary-search-tree.js) of the @codevolution YouTube video series):
```js
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  isEmpty() {
    return this.root === null;
  }

  insert(value) {
    const newNode = new Node(value);
    if (this.isEmpty()) {
      this.root = newNode;
    } else {
      this.insertNode(this.root, newNode);
    }
  }

  insertNode(root, newNode) {
    if (newNode.value < root.value) {
      if (root.left === null) {
        root.left = newNode;
      } else {
        this.insertNode(root.left, newNode);
      }
    } else {
      if (root.right === null) {
        root.right = newNode;
      } else {
        this.insertNode(root.right, newNode);
      }
    }
  }

  search(root, value) {
    if (!root) {
      return false;
    }
    if (root.value === value) {
      return true;
    } else if (value < root.value) {
      return this.search(root.left, value);
    } else {
      return this.search(root.right, value);
    }
  }

  min(root) {
    if (!root.left) {
      return root.value;
    } else {
      return this.min(root.left);
    }
  }

  max(root) {
    if (!root.right) {
      return root.value;
    } else {
      return this.max(root.right);
    }
  }

  delete(value) {
    this.root = this.deleteNode(this.root, value);
  }

  deleteNode(root, value) {
    if (root === null) {
      return root;
    }
    if (value < root.value) {
      root.left = this.deleteNode(root.left, value);
    } else if (value > root.value) {
      root.right = this.deleteNode(root.right, value);
    } else {
      if (!root.left && !root.right) {
        return null;
      }
      if (!root.left) {
        return root.right;
      } else if (!root.right) {
        return root.left;
      }
      root.value = this.min(root.right);
      root.right = this.deleteNode(root.right, root.value);
    }
    return root;
  }

  inOrder(root) {
    if (root) {
      this.inOrder(root.left);
      console.log(root.value);
      this.inOrder(root.right);
    }
  }

  preOrder(root) {
    if (root) {
      console.log(root.value);
      this.preOrder(root.left);
      this.preOrder(root.right);
    }
  }

  postOrder(root) {
    if (root) {
      this.postOrder(root.left);
      this.postOrder(root.right);
      console.log(root.value);
    }
  }

  levelOrder() {
    /** Use the optimised queue enqueue and dequeue from queue-object.js instead.
     * I've used an array for simplicity. */
    const queue = [];
    queue.push(this.root);
    while (queue.length) {
      let curr = queue.shift();
      console.log(curr.value);
      if (curr.left) {
        queue.push(curr.left);
      }
      if (curr.right) {
        queue.push(curr.right);
      }
    }
  }

  height(node) {
    if (!node) {
      return 0;
    } else {
      const leftHeight = this.height(node.left);
      const rightHeight = this.height(node.right);
      return Math.max(leftHeight, rightHeight) + 1;
    }
  }

  printLevel(node, level) {
    if (!node) {
      return;
    }
    if (level === 1) {
      console.log(`${node.element} `);
    } else if (level > 1) {
      this.printLevel(node.left, level - 1);
      this.printLevel(node.right, level - 1);
    }
  }

  isBST(node, min, max) {
    if (!node) {
      return true;
    }
    if (node.value < min || node.value > max) {
      return false;
    }
    return (
      this.isBST(node.left, min, node.value) &&
      this.isBST(node.right, node.value, max)
    );
  }
}

// TODO level order and delete

const bst = new BinarySearchTree();
console.log(bst.isEmpty());
bst.insert(10);
bst.insert(5);
bst.insert(15);
bst.insert(3);
bst.insert(7);
bst.insert(13);
bst.insert(17);
bst.insert(2);
console.log(bst.search(bst.root, 10));
console.log(bst.search(bst.root, 7));
bst.inOrder();
bst.preOrder();
bst.postOrder();
bst.levelOrder();
bst.printLevel(bst.root, 3);
console.log(bst.min());
console.log(bst.max());
console.log(bst.height(bst.root));
```
13. Graph Data Structure ([source](https://www.youtube.com/watch?v=bLtm94mvfjE&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=75))
- NOTES:
- I noticed There's multiple exercises Adjacency Matrix of a Graph; Adjacency List of a Graph; Graph Add Vertex and Edge; Graph Display and HasEdge; Graph Remove Edge and Vortex.
- Anyways, notes from the YouTube [video](https://www.youtube.com/watch?v=bLtm94mvfjE&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=75) by @codevlution:
- A graph is a non-linear Data Structure that consists of a finite number of Vertices -> also called Nodes -> connected by Edge.
- Trees themselves are a specific type of Graph Data Structure (*hm, that confuses me*).
- The graph has a set of Vetices (/Nodes) represented as Cricles and a set of Edges represented as Lines. -> As opposed to a Tree which is generally represented OR Red Top Down, instead there is **n** Hierarchy in Graphs and there is **no** Set way to represent or read a Graph.
- Visual representations (visual examples):
```
                   B
 (A points at B) /  \ (B points at C)
                A    C
```
- Types of Graphs (2 types of graphs):
	- #1 Directed
		- A Graph in which the Edges have a direction.
		- Edges are usually represented by arrows pointing in the direciton the graph can be traversed.
		  - In the example above -> we can **only traverse** from `A` -> `B` -> `C` ; but we **can't** traverse from `C` -> `B` -> `A`!
	- #2 Undirected
	  - A Graph in which the edges are biderecitonal.
      - The Graph can be traversed in either direction.
      - The absence of an arrow tells us taht the Graph is undirected.
        - In the example above we can traverse from both `A` -> `B` -> `C` as well as `C` -> `B` -> `A`.
- There's even **MORE TYEPS of Graphs!** Which I, of course, won't post them visually, here's the [YouTube video](https://youtu.be/bLtm94mvfjE?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=125) by @codevolution.
  - Here's their names for CTRL+F lookup:
  - Only vertices with no edges.
  - Multiple edges from 1 node.
  - Cycles in the Graph.
  - Self-loops on a Node.
  - "Maybe disconnected".
  - May contain weights on Edges representing the costs of traversing that Edge.
  - "*We will not be diving into implementing all of these variants (types) of Graphs*".
- Graph Usages in real life:
  - Google Maps
    - Where cities are represented as Vertices (*or Nodes*) and Roads as Edges -> to help build a Navigation system.
  - In social media apps / websites -> Users are considered as Vertices (*/Nodes*) and Edges are the links between Connections (linkings).
    - Example: facebook, instagram, linkedin all use Graph Data Structures to show **mutual connecitons,** **posts suggestions** and other **recommendations.** (*FAANGs examples use this Data Structure: Graphs, nicey!;* Yet a YouTube comment guy wrote that he was **never** asked about Graphs during his FAANG interviews (*I can't recall the video, yet it doesn't even matter, each to their own experience I may have it come up during an interview (the comment was posted like 5 years ago when I read it on June 2023 and went like "still in college" and then in the replies people asked him "what happened now" and the guy replied he was "hired working with Unity" & didn't mention company name; comment was maybe at the [YouTube Linked List by HackerRank](https://www.youtube.com/watch?v=njTh_OwMljA) or [YouTube Hash Tables by HackerRank](https://www.youtube.com/watch?v=njTh_OwMljA).).).).
### Graph Data Structure implementations in code:
```js
class Graph {
  constructor() {
    this.adjacencyList = {};
  }

  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) {
      this.adjacencyList[vertex] = new Set();
    }
  }

  addEdge(vertex1, vertex2) {
    if (!this.adjacencyList[vertex1]) {
      this.addVertex(vertex1);
    }
    if (!this.adjacencyList[vertex2]) {
      this.addVertex(vertex2);
    }
    this.adjacencyList[vertex1].add(vertex2);
    this.adjacencyList[vertex2].add(vertex1);
  }

  removeEdge(vertex1, vertex2) {
    this.adjacencyList[vertex1].delete(vertex2);
    this.adjacencyList[vertex2].delete(vertex1);
  }

  removeVertex(vertex) {
    if (!this.adjacencyList[vertex]) {
      return;
    }
    for (let adjacentVertex of this.adjacencyList[vertex]) {
      this.removeEdge(vertex, adjacentVertex);
    }
    delete this.adjacencyList[vertex];
  }

  hasEdge(vertex1, vertex2) {
    return (
      this.adjacencyList[vertex1].has(vertex2) &&
      this.adjacencyList[vertex2].has(vertex1)
    );
  }

  display() {
    for (let vertex in this.adjacencyList) {
      console.log(vertex + " -> " + [...this.adjacencyList[vertex]]);
    }
  }
}

const graph = new Graph();
graph.addVertex("A");
graph.addVertex("B");
graph.addVertex("C");
graph.addEdge("A", "B");
graph.addEdge("A", "C");
graph.addEdge("B", "C");
graph.display();
graph.removeEdge("A", "B");
graph.display();
graph.removeVertex("A");
graph.display();
```
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
#### Another confusion of mine clarified: calling multiple O(n) methods in a single function: still remains the same worst case scenario of O(n), however since I have multiple consecutive `map` calls in my code below, the **overall time complexity would be O(k * n),** where `k` is the number of consecutive `map` calls.
- So that's a Time Complexity I've never seen before!
```js
function random(arr) {
  arr.map();
  arr.map();
  arr.map();
  arr.map();
  arr.map();
};
```
---
My most helpful JavaScript based DSA course on YouTube by @Codevolution:
#### https://www.youtube.com/playlist?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP
NOTE#1: There's mistake in his Fibonacci Sequence so I'm fixing it on my own. His mistakes are both in his video: https://www.youtube.com/watch?v=tQjd29Rmo_A&list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&index=7 where his `for` loop has a wrong condition of `i < n` instead of `i <= n` like his correct verison at https://replit.com/@Codevolution/JavaScript-Algorithms#math/fibonacci.js -> which however he kept the *wrong comments* but the code is fixed otherwise.
  - As well as Recursive Fibonacci Sequence mistakes: https://youtu.be/wZNxLwqxu00?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP&t=55 here he correctly shows expected returned value BUT at https://replit.com/@Codevolution/JavaScript-Algorithms#math/fibonacci-recursive.js -> he mistakenly changed argument of 3rd call from `6` to `7` but wrong expected result `8` in a comment instead should be `13`, because result `8` corresponds to Fibonacci of `6`.

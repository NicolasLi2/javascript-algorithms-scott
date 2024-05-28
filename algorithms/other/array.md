### ARRAY

#### Sorted Squared Array

You are given an array of Integers in which each subsequent value is not less than the previous value. Write a function that takes this array as an input and returns a new array with the squares of each number sorted in ascending order.

##### Clarifying questions:

- Can the input array be empty? - yes
- Can the input array contain negative numbers? - yes
- Can the input array contain duplicates? - yes

```js
// Method 1
// Time: O(nlogn) - Space: O(n)
function sortedSquared(array) {
  const newArray = new Array(array.length).fill(0); // Space: O(n)
  // Time: O(n)
  for (let i = 0; i < array.length; i++) {
    newArray[i] = Math.pow(array[i], 2);
  }
  newArray.sort((a, b) => a - b); // Time: O(nlogn)
  return newArray;
}

// Another solution
function sortedSquared(array) {
  const newArray = array.map((item) => item ** 2).sort((a, b) => a - b);
  return newArray;
}

// Method 2 - Two pointers: Make use of the fact that the given array is sorted in ascending order
// Time: O(n) - Space: O(n)
function sortedSquared(array) {
  const newArray = new Array(array.length).fill(0);
  let pointerLeft = 0;
  let pointerRight = array.length - 1;
  for (let i = array.length - 1; i >= 0; i--) {
    if (Math.abs(array[pointerLeft]) > Math.abs(array[pointerRight])) {
      newArray[i] = array[pointerLeft] ** 2;
      pointerLeft++;
    } else {
      newArray[i] = array[pointerRight] ** 2;
      pointerRight--;
    }
  }
  return newArray;
}

let a = [1, 3, 4, 5];
let b = [-7, -2, 3, 4, 9];
let c = [];

console.log(sortedSquared(a)); // [1, 9, 16, 25]
console.log(sortedSquared(b)); // [4, 9, 16, 49, 81]
console.log(sortedSquared(c)); // []
```

#### Monotonic Array

An array is monotonic if it is either monotone increasing or monotone decreasing. An array is monotone increasing if all its elements from left to right are non-decreasing. An array is monotone decreasing if all its elements from left to right are non-increasing. Given an integer array return true if the given array is monotonic, or false otherwise.

##### Clarifying questions:

- Is an empty array considered monotonic? - yes
- Is an array with only 1 integer considered monotonic? - yes

```js
// Time: O(n) - Space: O(1)
function checkMonotonic(array) {
  const first = array[0];
  const last = array[array.length - 1];

  if (first === last) {
    for (let i = 0; i < array.length - 1; i++) {
      if (array[i] !== array[i + 1]) return false;
    }
  } else if (first < last) {
    // Increasing
    for (let i = 0; i < array.length - 1; i++) {
      if (array[i] > array[i + 1]) return false;
    }
  } else {
    // Decreasing
    for (let i = 0; i < array.length - 1; i++) {
      if (array[i] < array[i + 1]) return false;
    }
  }
  return true;
}

let a = [1, 2, 2, 3];
let b = [6, 5, 4, 4];
let c = [1, 2, 3, 1, 4];
let d = [];
let e = [4];

console.log(checkMonotonic(a)); // true
console.log(checkMonotonic(b)); // true
console.log(checkMonotonic(c)); // false
console.log(checkMonotonic(d)); // true
console.log(checkMonotonic(e)); // true
```

#### Rotate Array

Given an array, rotate the array to the right by k steps, where k is non-negative. Eg. [1, 2, 3, 4, 5, 6, 7] and k = 3, the output should be [5, 6, 7, 1, 2, 3, 4].

##### Clarifying questions:

- What happens if an empty array is given? - return the empty array
- If k=0, no rotation is to happen? - yes

```js
// Method 1 - Time O(n) - Space O(n)
const rotateArray = function (array, k) {
  const length = array.length;
  k = k % length;
  const i = length - k;

  const left = array.slice(0, i);
  const right = array.slice(i);
  return right.concat(left);
};

// Method 2 - Time O(n) - Space O(1)
const rotateArray = function (array, k) {
  k = k % array.length;
  reverse(0, array.length - 1);
  reverse(0, k - 1);
  reverse(k, array.length - 1);

  function reverse(start, end) {
    while (start < end) {
      [array[start], array[end]] = [array[end], array[start]];
      start++;
      end--;
    }
  }

  return array;
};

let a = [1, 2, 3, 4, 5, 6, 7]; // length = 7
console.log(rotateArray(a.slice(), 3)); // [5, 6, 7, 1, 2, 3, 4] // using a.slice() to make a copy of the array, because the method 2 modifies the original array.
console.log(rotateArray(a.slice(), 100)); // [6, 7, 1, 2, 3, 4, 5] - 100 % 7 = 2
console.log(rotateArray([], 2)); // []
console.log(rotateArray([1], 2)); // [1]
```

#### Container with most Water

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).
Find two lines that together with the x-axis form a container, such that the container contains the most water(depth is constant across containers). Return the area(that the 2 lines and the X axis make) of container which can store the max amount of water. Notice that you may not slant the container.

##### Clarifying questions:

- Dose Y axis count as a wall? - No
- Does a line inside the container affect the area? - No

![image](./images/Container-with-most-Water.webp)

![Container with most water](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

```js
// Method 1 - Brute force
// Time: O(n^2) - Space: O(1)
const maxArea = function (array) {
  let area = 0;
  for (let i = 0; i < array.length - 1; i++) {
    for (let j = i + 1; j < array.length; j++) {
      const width = j - i;
      const height = Math.min(array[i], array[j]);
      area = Math.max(area, width * height);
    }
  }
  return area;
};

// Method 2 - Two pointers
// Time: O(n) - Space: O(1)
const maxArea = function (array) {
  let area = 0;
  let left = 0;
  let right = array.length - 1;
  while (left < right) {
    const width = right - left;
    const height = Math.min(array[left], array[right]);
    area = Math.max(area, width * height);
    if (array[left] < array[right]) left++;
    else right--;
  }
  return area;
};

let a = [1, 8, 6, 2, 5, 4, 8, 3, 7]; // length = 9
let b = [3, 7, 5, 6, 8, 4]; // length = 6

console.log(maxArea(a)); // 49
console.log(maxArea(b)); // 21
```

### HASH TABLE

#### Two Sum

You are given an array of Integers and another integer targetValue. Write a function that will take these inputs and return the indices of the 2 integers in the array that add up targetValue.

##### Clarifying questions:

- What if no two integers add up to the targetValue? - return an empty array
- Will multiple pairs add up to the targetValue? - no, at most 1 pair
- Are all the integers positive? - no
- Are the integers distinct? - yes
- What if the array is empty? - return an empty array
- Can I add the same number twice? - no

```js
// Method 1 - Brute force
// Time: O(n^2) - Space: O(1)
function findIndicesSum(array, targetValue) {
  for (let i = 0; i < array.length - 1; i++) {
    for (let j = i + 1; j < array.length; j++) {
      if (array[i] + array[j] === targetValue) {
        return [i, j];
      }
    }
  }
  return [];
}

// Method 2 - Hash table
// Time: O(n) - Space: O(n)
function findIndicesSum(array, targetValue) {
  const hashTable = {};
  for (let i = 0; i < array.length; i++) {
    const neededValue = targetValue - array[i];
    if (neededValue in hashTable) {
      return [hashTable[neededValue], i];
    }
    hashTable[array[i]] = i;
  }
  return [];
}

let a = [2, 7, 11, 15];
let b = [3, 2, 4];
let c = [4];
let d = [];

console.log(findIndicesSum(a, 9)); // [0, 1]
console.log(findIndicesSum(b, 6)); // [1, 2]
console.log(findIndicesSum(c, 4)); // []
console.log(findIndicesSum(d, 4)); // []
```
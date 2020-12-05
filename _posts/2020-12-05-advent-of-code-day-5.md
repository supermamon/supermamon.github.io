---
layout: post
title: "Advent of Code Day 5"
categories: [programming]
tags: scriptable, javascript, adventofcode
comments: true
---

You learn something new everyday. And there's done one way to arrive to a solution for any problem. 

<!--more-->

**Day 5**

The challenge: To me, the problem seems like finding the edge of a fixed-depth binary-tree. But it's much simpler actually. Read the [actual challenge](https://adventofcode.com/2020/day/5).

When I first read the problem, I thought, that isn't too hard. Split the text into it's 7 and 3-letter halves and create a recursive function to calculate each value -- which is what I did.

Here's the function:

```javascript
function findBinaryEnd(path, start, end) {
  if (!path) return start
  const next = path.match(/^./)[0]
  if (!/[01]/.test(next)) return start
   
  let newStart,newEnd
  if (next == '0') {
    newStart = start
    newEnd   = ((end+1)-start)/2 + start-1
  } else {
    newStart = ((end+1)-start)/2 + start 
    newEnd   = end
  }
  return findBinaryEnd(path.split('').slice(1).join(''), newStart, newEnd)
}
```

The functions accepts 3 inputs, a binary number, start and end range. Example call would be:

```javascript
findBinaryEnd('0101100', 0, 127) // yields 44
```

My sample input is `FBFBBFFRLR` which I split into the first 7 and last 3 characters that represent `row` and `column`. I did this because I was thinking that I need the individual values for the row and colum, based on how the problem was described. To call my recursive function, I replace `F` and `L` with `0` and `B` and `R` with `1`. It yielded me with the numbers I needed and was able to answer Part 1 of the problem.



**Part 2**

The way it was worded threw me off a bit. The way I understood it was to find the missing integer in a range of integers but don't include the first and the last. Why do I need to exclude them? The numbers are consecutive so the first or the last one is only +/- different from prior one. And... Oh wait, ahh. That's why. Including them meant the answer could be either of them so, yes that's it.

*Lesson 1*.  Javascript's `Array.sort()` doesn't sort numbers as numbers. 

I had always assumed that that `Array.sort()` sorts strings alphabetically (case-sensitive) and numbers, well, numerically. I was wrong.

When I checked the sorted array on the console, I was stumped. Further checks and a little [research](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) confirms that yes, it indeed sorts as strings.  I made the adjustment and moved on.

With the sorting in place, I was able to find the missing number from the list.



*Lesson 2*. There's more than one way to solve the problem. Sometimes it's simpler than you think.

Now that I have sumitted my answers, time to review the code. First one on the grill is the `findBinaryEnd` function. Is there a better way to do this? The more I thought about it, the more i realized the input is simply a binary number. And all I have to do was to convert it to decimal. I didn't need the row and the column values.

But I didn't want to reinvent the wheel and just instad find a binary to decimal converter. Which lead me to our trusty [Stack Overflow](https://stackoverflow.com/questions/10258828/how-to-convert-binary-string-to-decimal). 

```javascript
var digit = parseInt(binary, 2);
```

Woah! `parseInt` has a second parameter? This is nuts.  The whole `seatIds` calculation could've been just a single line.

```javascript
const seatIds = passes.map( p => parseInt(p.replace(/[FL]/g,0).replace(/[BR]/g,1),2))
```



See the complete code at [aoc-2020](https://github.com/supermamon/aoc-2020).


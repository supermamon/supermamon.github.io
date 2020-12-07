---
layout: post
title: "Advent of Code Day 6"
categories: [programming]
tags: scriptable, javascript, adventofcode
comments: true

---

Today's challenge presents two concepts - finding unique values and set intersection. Finding unique values in part 1 wasn't that difficut but it took me a while to figure out the intersection of multiple arrays.

<!--more-->

**Day 6**

The challenge: The [challenge](https://adventofcode.com/2020/day/6) was presented as a set of yes/no questions that is answered by several groups of people. Each person in the group lists the question *letter* where the answe is yes. Part 1 asks to count all the questions where the group collectively answered. Part 2 asks the number of questions that all members or the group answered yes to.

Part one is quite straightforward. What I did was to collect every members answers into an array and then remove duplicates, i.e. find only the uniqe values.

Here's the long version.

```javascript
const groups = input.split("\n\n") 
const grpAns = groups.map( memberAns => memberAns.replace(/\n/g,'') )
const unique = grpAns.map( allAns => {
  return allAns.split('').filter( (q,i,l) => l.indexOf(q)==i ).length
})
const questions = unique.reduce( (a,c) => a+c )

```

And the short one just doing away with the intermediate variables.

```javascript
const questions = input.split("\n\n").map( g => g.replace(/\n/g,'') ) 
.map( a => {
  return a.split('').filter( (q,i,l) => l.indexOf(q) == i ).length
}).reduce( (a,c) => a+c )

```



**Part 2**

To be honest, I read the first part fast enough to miss some details on how the input was actually structured. Specifically the fact that each line represents answers from one person. So, I had tough time understanding what *identify the questions to which everyone answered "yes"* meant. 

So I went back and re-read the problem. And it hit me! It's a matter of finding the common letters from all the answers from each group. The solution I have settled is to start with the answers from the first person, then filtering down the common answers from the succeeding persons. What's left would be all the letters in common with everybody.

Here's that translated into code:

```javascript
input.split("\n\n")
.map( g => {
  let q = []
  g.split("\n").forEach( (p,i) => {
    q = i == 0 ? p.split('') : p.split('').filter( a => q.indexOf(a) >= 0 )
  })
  return q.length
})
.reduce( (a,c) => a+c )

```



See the complete code at [aoc-2020](https://github.com/supermamon/aoc-2020).


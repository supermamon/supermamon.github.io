---
layout: post
title: "Advent of Code Day 1"
categories: [scriptable]
tags: scriptable, javascript
comments: false

---

So I saw a bunch of people tweeting about [#AdventOfCode](https://twitter.com/hashtag/AdventOfCode). I remember seeing that tag before but I never bothered to actually see what is it all about. My curious side hit me today so I went ahead to join.

<!--more-->

I will not go into detail on what the [#AdventOfCode](https://twitter.com/hashtag/AdventOfCode) is about but to make the long story short, it's a series of programming puzzles the go on for 25 days starting from Dec 1. There will be 2 puzzles everyday. You can read about it in detail on [adventofcode.com](https://adventofcode.com/2020/about).

**The Weapon of Choice: Scriptable**

I like Javascript. I has become ubiquitous in such a way that you could use it almose anywhere -- even on your phones. As of today, I when I think Javasript on a hand-held device, there's nothing better than [Scriptable](https://scriptable.app). 

Scriptable packs a lot of things you can access with Javascript. From native iOS APIs to Shortcuts and iCloud integration and recently - Widgets. Widgets on iOS/iPadOS have become very popular since their release on iOS/iPadOS 14. Scriptable has become one of best tools to author widgets. I have been actively using Scriptable have created a few widgets of [my own](https://github.com/supermamon/scriptable-scripts). So, it's the first thing that came into my mind when I though of joining the #AdventOfCode.

**Day 1**

The challenge: Given a list of numbers, *find the two entries that sum up to 2020* and then multiply those two numbers together.

My first thought. I could just loop through each number and add each one to the same set of numbers. 

But that dosn't work. You shouldn't adding the same number to itself. So, somehow, the current number should be removed from the equation. Here's my first version.

```javascript
const sums = input.map( (m) => {  
  let copy = input.filter( a => a!=m )
  return copy.map( n => {return {m,n, sum: m+n}} )
}).flat().filter( o => o.sum==2020 )

log(sums)  // [{"m":1236,"n":784,"sum":2020},{"m":784,"n":1236,"sum":2020}]

const {m,n} = sums[0]
const output = m * n

log(`
numbers: ${[m,n]}
output : ${output}
`)
// numbers: 1236,784
// output : 969024

```

My gripe with this version is that it `sums` has 2 sets of answers. It's because it's still adds up the numbers  that are in front of the `copy` variable, which have already been used before. So, why not skip those with `Array.slice` instead. And throw the filter earlier while we're at it.

```javascript

const sums = input.map( (m,i) => {  
  let copy = input.slice(i+1).filter( n => m+n==2020 )
  return copy.map( n => {return {m,n}} )
}).flat()

log(sums) // [{"m":1236,"n":784}]

const {m,n} = sums[0]
const output = m * n

log(`
numbers: ${[m,n]}
output : ${output}
`)
// numbers: 1236,784
// output : 969024

```



Ah, much better.  With this, the second puzzle of the day has been unlocked. It's not much diffrent. Instead of two numbers, it's looking for 3. 

For the curious, you can see all the code on my [aoc-2020](https://github.com/supermamon/aoc-2020) repo.






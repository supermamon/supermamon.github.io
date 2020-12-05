---
layout: post
title: "Advent of Code Day 2 - Passwords"
categories: [programming]
tags: scriptable, javascript, adventofcode
comments: true


---

Second day of coding puzzles - validating passwords. My first thought this would involve matching strings against each other but it was more than that.

<!--more-->

**Day 2**

The challenge: Given a list, containing a password policy on each item, count how many passwords are valid.

Woah! Different policies for every record? Well not really. The rule was the is the same for every record but that rule has parameters. You can read about the challenge [here](https://adventofcode.com/2020/day/2).

Here's how I solved it.

```javascript

const valid = input.filter( record => {
  
    // split the line to have the password policy and password as separate variable
    const r1 = record.split(':')
    const policyStr = r1[0]
    const passwd = r1[1].trimStart()
    
    // extract the parameters of the policy - range and the passowrd character  
    const policyR = policyStr.split(' ')
    const plyRng  = policyR[0].split('-').map(n=>parseInt(n))
    const plyChr  = policyR[1]

    // count the number of characters that match the policy    
    const count = passwd.split(plyChr).length-1
    
    // the count should be within the range.
    return (count >= plyRng[0] && count <= plyRng[1])
})
const output = valid.length
const content = `
output : ${output}
`
log(content)
// output : 645
```

The line that got me thinking would be how to count the matching characters.

```javascript
const count = passwd.split(plyChr).length-1
```

I initially thought about extracting the matching characters from the password. But that would take more steps than I wanted to. So, why not use the character to split the password. The size of resulting array should indicate the number of matching characters, minus 1. So I did just that and it worked accurately.

**Part 2** 

The second part gets more interesting. Instead of matching the number of characters we actually need to match the exact positions for the password to be valid. Adding more twist is that it should only match either one position but not both.  

```javascript
    ...
    const passwd = r1[1] //.trimStart()
    ...
	  const passArr = passwd.match(/./g)
    
    const matchBoth   = passArr[plyRng[0]]==plyChr && passArr[plyRng[1]]==plyChr
    const matchEither = passArr[plyRng[0]]==plyChr || passArr[plyRng[1]]==plyChr
    
    return ( !matchBoth && matchEither )
    
```

And extra twist about this challenge is that there arrays in Javascript start at index 0. The good thing is that for each record, the password is preceeded by a space. So, not trimming the section that contains the password actually aligns the index to that of the password policy.

```javascript
const passwd = r1[1] //.trimStart()
```

With that, the character matching can be applied to the designated index, without minding the 0/1 start index argument. 

As before, code is on my [aoc-2020](https://github.com/supermamon/aoc-2020) repo.






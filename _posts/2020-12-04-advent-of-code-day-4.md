---
layout: post
title: "Advent of Code Day 4"
categories: [programming]
tags: scriptable, javascript
comments: true




---

I sensing a pattern here. I might need to build a compiler soon. 

<!--more-->

**Day 4**

The challenge: Validate records in a database. The database is a text file with 1 or more key:value pairs on each line. A record can span multiple lines. The only separator between records is a blank line. Read more abou the details [here](https://adventofcode.com/2020/day/4).

The first part of the puzzles isn't that complicated. A valid record has 7 mandatory fields and 1 optional one. It would be easy enough to filter out the optional field and count if there are 7 remaining fields. But first, we have to solve the part where each record may in stored in multiple lines. Our trusty `Array.split()` and `Array.map()` comes in handy.

```javascript
const rows = input.split("\n\n")
     .map( r => r.replace(/\n/g," ") )
```

So what does this do? First `.split("\n\n")` creates an array of the individual records. Since there's a blank line separating each record, there will be 2 consecutive new line characters in between them. Then, knowing that each record might have been split into multiple lines, we remove those new line characters and replace each one with a space. Now we have each record on its own line.

At this point, I just wanted to split each line like what I did in [day 2](https://github.com/supermamon/aoc-2020/blob/master/AoC20-02a.js#L24), then filter out the one that begins with `cid` like so:

```javascript
const rowLengths = rows.map( row => {
  return row.split(' ').filter( kv => !(/^cid/.test(kv))).length
})
const valid = rowLengths.filter( len => len == 7 ) 
```

But I'm not trying to rank anyway so I'm not in a hurry. So I figured why not extract the actual key:value pairs. Thus, my solution:

```javascript
let valid = input
            .split("\n\n")
            .map( r => r.replace(/\n/g," ") )
            .filter( r=>{
              let j = r.split(' ')
                      .map( kv => {
                        kva = kv.split(':')
                        return {k:kva[0],v:kva[1]}
                      })
                      .filter( kv => kv.k != 'cid' )
              return j.length == 7  
            })
const output = valid.length
```

But hold up! I realized that this event this solution worked to give me the answer, it has a fault. It assumes that a record only contains valid keys. A record with 7 keys, whatever they are would pass the test. Adding another filter will solve that.

```javascript
...
.filter( kv => /^(byr|iyr|eyr|hgt|hcl|ecl|pid)$/.test(kv.k) )
return j.length == 7
```

**Part 2**

Here we go, Parsing out the keys and values is paying off. Part 2 now asks to validate those values. I thought of building a minimal rule engine - a dictionary of functions where the function names a the names of the valid keys.

```javascript
  // ** rule engine ** 
  // range check function
  const rge = (v,s,e) => { v=parseInt(v); return (v>=s && v<=e) }
 
  const rules={
    byr: v => rge(v,1920,2002),
    iyr: v => rge(v,2010,2020),
    eyr: v => rge(v,2020,2030),
    hcl: v => /^#[0-9a-f]{6}$/.test(v),
    ecl: v => /^amb|blu|brn|gry|grn|hzl|oth$/.test(v),
    pid: v => /^\d{9}$/.test(v),
    hgt: v => {
      // match format
      if (!(/\d+(cm|in)/.test(v))) {return false}
      
      // extract amount and unit
      const mv = v.match(/(\d+)(cm|in)/)
      if (mv.length==0) return false
      
      const am = parseInt(mv[1])
      if (mv[2]=='cm') {
        return am >= 150 && am <= 193
      }
      if (mv[2]=='in') {
        return am >= 59 && am <= 76
      }
      return false
    },
    cid: v => true    
  }
  // j = the records to check. an array of {k:key, v:value}
  const checks = j.map( kv => rules[kv.k] ? rules[kv.k](kv.v) : false)
  return checks.reduce( (a,c) => a && c )
```

There's a bit of a twist here. On part 1, I added a filter to only include the valid keys. I removed that and handed the job to the rule engine. What are the implications?

First, it was neither defined not implied whether the input will have extra keys other than the ones listed. And if there are any, would it make the record invalid if even contains all the necessary keys? If yes, adding that filter would make the solution wrong because it ignores any extraenous keys.

Second, the rule engine assumes that extra keys are invalid. Of course, that makes it a problem if they are not.

I kept that assumption for now and call it a day.

The complete code can be found at my [aoc-2020](https://github.com/supermamon/aoc-2020) repo.












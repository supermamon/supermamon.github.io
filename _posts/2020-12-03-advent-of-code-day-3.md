---
layout: post
title: "Advent of Code Day 3 - The Forest is Scary"
categories: [scriptable]
tags: scriptable, javascript
comments: true



---

Day number 3. It wasn't a walk in the park. We're talking about a forest here. *Spoiler: I cheated*.

<!--more-->

**Day 3**

The challenge: You are given a grid. Each block on the grid is either empty or a tree. You start from the top left and are given a slope in terms of column and row steps - example 3 steps right and 1 step down. Everytime you land on a block, keep a running total if you land on a tree until you reach the bottom of the grid. The complication is that the grid is taller than it's wide. So you have to continue the step back to the left when you reach the right edge.

I presumed that the slope will be a variable on the second puzzle so I kept that in mind.

```javascript
let rows = input
function countTrees(rows, coldelta, rowdelta) {
  const rowc  = rows.length, 
        colc  = rows[0].length,
        slope = Math.ceil( coldelta/rowdelta )
    
  const cfactor = Math.ceil(rowc * slope / colc )

  if (cfactor>1) {
    // cheat
    rows = input.map( row => row.repeat(cfactor) )
  }

  let irow=0,icol=0,trees=0
      
  while (irow < rowc) {
    icol += coldelta
    irow += rowdelta
    
    if (irow>rowc-1) break;
    
    const rowA = rows[irow].split('')
    trees += rowA[icol]=='#' ? 1 : 0
  }
  return trees
} 

const output = countTrees(rows, 3, 1 )
// output : 220

```

Did I mentioned I cheated? 

```javascript
rows = input.map( row => row.repeat(cfactor) )
```

Yes, this shows that instead of building a logic to go back to the left side of the grid, I expanded the grid to repeat the row as many times as needed. But to be fair, that's how the website has presented it - the pattern repeats.

For part 2, like I expected, the slope is variable. But instead of just returning the number of trees, it ask to multiply those results together. This is a good chance to use `Array.reduce()`. Below is how the last part of the code looks like.

```javascript
...
const slopes = [ [1,1], [3,1], [5,1], [7,1], [1,2] ]
const trees = slopes.map( slope => { 
   return countTrees(rows, slope[0], slope[1])
})

const output = trees.reduce( (a,c) => a * c )

// trees  : 70,220,63,76,29
// output : 2138320800

```

You can check the rest of the code on my [aoc-2020](https://github.com/supermamon/aoc-2020) repo.






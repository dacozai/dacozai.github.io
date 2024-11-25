---
layout: post
title: A Little Note about Algorithm in Rust
date: 2024-11-24 10:05:00+0800
description: a note for you and me to crack algorithm in Rust
tags: rust algorithm
categories: note
giscus_comments: true
featured: true
related_posts: false
toc:
  beginning: true
---

This post is **an algorithm note** to me but I hope this could help others too. My problem source is from <a href="https://leetcode.com/">LeetCode</a>, which is nothing but simple.

# CheatSheet

In this section, some useful in-built or template like solution will be listed here. Besides, I will further clarify some misunderstood concepts for people to review before attending the coding interview.

```rust
/// Sort
/// Reference -- https://doc.rust-lang.org/std/primitive.slice.html#method.sort_unstable
/// There are typcial and useful tips
let arr = vec![4, -5, 1, -3, 2];
// NOTE: DEFAULT ordering is **ASC**
arr.sort_unstable(); // [-5, -3, 1, 2, 4]
arr.sort_unstable_by(|a,b| b.cmp(a)); // reverse sorting -> [4, 2, 1, -3, -5]
// Customised case 1
let mut nums = vec![3, 30, 34, 5];
nums.sort_unstable_by(|&s, &t| {
    let one = format!("{s}{t}");
    let two = format!("{t}{s}");
    two.cmp(&one)
}); // Output: [5, 34, 3, 30]
// Customised case 2
let mut stk = vec![
    (10, 2, 3, 4),
    (10, 2, 3, 1),
    (5, 8, 9, 2),
    (10, 1, 5, 3),
    (10, 2, 1, 3),
];
stk.sort_unstable_by(|a, b| {
    b.0.cmp(&a.0)                     // 1. Sort `b.0` descending (reverse order of `a.0` and `b.0`)
        .then_with(|| b.1.cmp(&a.1))  // 2. If `b.0 == a.0`, sort `b.1` descending
        .then_with(|| b.2.cmp(&a.2))  // 3. If `b.1 == a.1`, sort `b.2` descending
        .then_with(|| a.3.cmp(&b.3))  // 4. If `b.2 == a.2`, sort `a.3` ascending
});
// Output
// [
//     (10, 2, 3, 1),
//     (10, 2, 3, 4),
//     (10, 2, 1, 3),
//     (10, 1, 5, 3),
//     (5, 8, 9, 2),
// ]

/// Binary Search
/// Reference -- https://doc.rust-lang.org/std/primitive.slice.html#method.binary_search
/// Case 1. locate the position of the given *value*
///      0  1  2  3  4  5  6  7  8  9   10  11  12
let s = [0, 1, 1, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55];
let low = s.partition_point(|x| x < &1);
assert_eq!(low, 1);
let high = s.partition_point(|x| x <= &1);
assert_eq!(high, 5);
let r = s.binary_search(&1);
assert!((low..high).contains(&r.unwrap()));
/// Case 2. Not Found
assert_eq!(s.partition_point(|x| x < &11), 9);
assert_eq!(s.partition_point(|x| x <= &11), 9);
assert_eq!(s.binary_search(&11), Err(9));
/// Case 3. Customised condition
let s = [0, 1, 1, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55];
let seek = 13;
assert_eq!(s.binary_search_by(|probe| probe.cmp(&seek)), Ok(9));
/// typical usage
let ss = [0, 1, 1, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55];
let target = 4;
let the_index = ss.binary_search(&target).unwrap_or_else(|insert_index| insert_index);
//if not found can be inserted into
ss.insert(the_index, 4);
```

# Algorithms

The backbone is based on the interview crash course, <a href="https://leetcode.com/explore/featured/card/leetcodes-interview-crash-course-data-structures-and-algorithms/">Data Structures and Algorithms</a>, from LeetCode. Yes, I recommend this course when you are busy at work and you need a proper way to learn algorithm gradually. Like I said, this is a post about the techniques and remiders to myself. Therefore, it might look incoherent to you unlike the course.

But, I will list many useful techniques and why I failed to solve some problems and what was I thinking atm.

## Array and strings

### Array

### String

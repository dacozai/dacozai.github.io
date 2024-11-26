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

/// Match tips
// match with the condition
#[allow(dead_code)]
enum Temperature {
    Celsius(i32),
    Fahrenheit(i32),
}
let temperature = Temperature::Celsius(35);
match temperature {
    Temperature::Celsius(t) if t > 30 => println!("{}C is above 30 Celsius", t),
    // The `if condition` part ^ is a guard
    Temperature::Celsius(t) => println!("{}C is equal to or below 30 Celsius", t),
    Temperature::Fahrenheit(t) if t > 86 => println!("{}F is above 86 Fahrenheit", t),
    Temperature::Fahrenheit(t) => println!("{}F is equal to or below 86 Fahrenheit", t),
}
// match with binding
let age = 15;
match age() {
    0             => println!("I haven't celebrated my first birthday yet"),
    n @ 1  ..= 12 => println!("I'm a child of age {:?}", n),
    n @ 13 ..= 19 => println!("I'm a teen of age {:?}", n),
    n             => println!("I'm an old person of age {:?}", n),
}
// >>> Tell me what type of person you are
// >>> I'm a teen of age 15
let some_num = Some(42);
match some_number() {
    Some(n @ 42) => println!("The Answer: {}!", n),
    Some(n)      => println!("Not interesting... {}", n),
    _            => (),
}
// >>> The Answer: 42! 

// let ... else early return
fn dfs(head: &Option<Box<ListNode>>) -> bool {
    // this is called let ... else syntax for early return, which is valid after Rust 1.65
    // reference -- https://doc.rust-lang.org/rust-by-example/flow_control/let_else.html
    let Some(hh) = head else { return true };
    false
}

/// For loop
/// 
/// 1. loop for reference reading
/// for name in names.iter() 
/// 2. loop and then consume the data source, meaning that elements are moved out from the array 
/// for name in names.into_iter() 
/// 3. loop for mut reference, so that we can modify the element directly
/// for name in names.iter_mut() 
```

# Algorithms

The backbone is based on the interview crash course, <a href="https://leetcode.com/explore/featured/card/leetcodes-interview-crash-course-data-structures-and-algorithms/">Data Structures and Algorithms</a>, from LeetCode. Yes, I recommend this course when you are busy at work and you need a proper way to learn algorithm gradually. Like I said, this is a post about the techniques and remiders to myself. Therefore, it might look incoherent to you unlike the course.

But, I will list many useful techniques and why I failed to solve some problems and what was I thinking atm.

## Array and strings

### Array

### String

## Linked List
In this section, it is difficult when using Rust because there are more restrictions placed on flow manipulation.
```rust
/// Exalpme Problem - https://leetcode.com/problems/add-two-numbers/description/
pub fn exec_function(l1: Option<Box<ListNode>>, l2: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
    // Initialize a dummy node to act as the base of the resulting list
    let mut base = ListNode { val: 0, next: None };
    // Get a mutable reference to the dummy node to act as our cursor
    let mut cursor = &mut base;

    // Use `as_ref` to get references to the inner data of `Option<Box<ListNode>>`.
    // Without `as_ref`, unwrapping would consume the `Option` and move its value, 
    // which we don't want as we only need references.
    let mut cur_l1 = l1.as_ref();
    let mut cur_l2 = l2.as_ref();
    let mut carry = 0i32;

    // Iterate as long as there are nodes in either list or a carry remains
    while cur_l1.is_some() || cur_l2.is_some() || carry > 0 {
        let mut next_val = carry; // Start with the carry from the previous operation
        if let Some(node) = cur_l1 {
            next_val += node.val; // Add the value from the current node in `l1`
            cur_l1 = node.next.as_ref(); // Move to the next node in `l1`
        }
        if let Some(node) = cur_l2 {
            next_val += node.val; // Add the value from the current node in `l2`
            cur_l2 = node.next.as_ref(); // Move to the next node in `l2`
        }

        // Create a new node for the current sum (mod 10)
        let the_node = ListNode { val: next_val % 10, next: None };
        carry = next_val / 10; // Update the carry for the next iteration

        // Assign the new node to `cursor.next` and move the cursor forward
        cursor.next = Some(Box::new(the_node));
        cursor = cursor.next.as_mut().unwrap(); // Advance the cursor to the newly added node
    }

    base.next // Return the resulting list, skipping the dummy node
}
```

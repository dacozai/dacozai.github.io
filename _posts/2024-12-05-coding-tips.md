---
layout: post
title: A Little tips and lessons learned from people
date: 2024-12-05 15:14:00+0800
description: this is a continous updated note that I learn from experience of mine or others
tags: rust
categories: experience
giscus_comments: false
featured: true
related_posts: false
---

## Match Problem

Here is the original [post](https://users.rust-lang.org/t/this-match-feature-is-awful/122046/38). Basically, the pain point is that people want to use the match arm but, somehow, OP encountered either typo or non-existed matching. Rust did not alert to the OP. Therefore, it becomes very difficult to find out the problem.

```rust
#[test]
fn awful() {
    enum Metal {
        Iron, Gold, Silver,
    }

    use Metal::*;

    let metal = Silver;

    let is_gold = match metal {
        Iron => false,
        God => true, //<-typo, warning: unused variables
        Awful => false, //warning: unreachable patterns
        _ => false, //useless, warning: unreachable patterns
    };

    assert!(!is_gold); //panic: is_gold == true
}
```

### Why

Based on the [Match Arm documentation](https://doc.rust-lang.org/book/ch06-02-match.html#catch-all-patterns-and-the-_-placeholder), the next non-matching item will become the so-called `catch-all-patterns`. Sometimes, we use `_` as the non-binding but match all. But, we can actually use a variable to represent as the match-all. Therefore, it stucks at **God** item which return true.

### Solution

```rust
enum Foo {
    A,
    B,
}

fn foo(foo: Foo) {
    use Foo::*;

    let x = 123; // only a warning

    #[deny(unused_variables)]
    match foo {
        A => println!("A"),
        C => println!("B"), // compilation fails with error
    }
}
```

As the doc mentioned, when we declare to catach-all, it is a binded variable to receive the match arm result. Therefore, we can use the attr called `#[deny(unused_variables)]` to force MUST-use of the variable. This is an actual brilliant workaround to solve this pain point!

---
layout: post
title: A fundamental guide
date: 2024-12-02 20:08:00+0800
description: a note for you and me to crack algorithm in Rust
tags: rust basic
categories: note
giscus_comments: true
featured: true
related_posts: false
toc:
  beginning: true
---

# Fundamental Traits
This note wants to guide you to understand these traits and other useful methods that you might encounter frequently. 
There are many fundamental and useful traits in Rust. It is important to understand then and, in the future, this can help you to understand what others' code are writing about.

# Traits
## Clone && Copy
Probably, the first time learning Rust, these two are the first two that you will encounter. They have more things to talk about but we currently focus on more basic stuffs.
### Clone
[Reference](https://doc.rust-lang.org/std/clone/trait.Clone.html) - There are two methods in this trait
```rust
fn clone(&self) -> Self;
fn clone_from(&mut self, source: &Self) { ... }
```
As mentioned in the documentation, `Clone` is always **explicit** and may or may not be **expensive**. Remember that, `Clone` is more `general` than `Copy`. 

Additionally, `Clone` cannot be applied on the dyn trait directly because dyn requires the objects to be [object-safe](https://doc.rust-lang.org/std/keyword.dyn.html#fnref1)! The workaround here is adding the extra object safe object to contain the struct. It can be Box, Rc, or else. Here is the [example](https://stackoverflow.com/a/30353928). There is a crate called [dyn-clone](https://github.com/dtolnay/dyn-clone) which will be your useful tool as well.

### Copy
[Reference](https://doc.rust-lang.org/std/marker/trait.Copy.html) - Mentioned in Clone section, Copy is more restrictive form. `Copy` is just a `Marker trait` that derives `Clone`. The difference here is that, Copy only handles `T` that can be **bit-wise copied** and it happens **implicitly**. 
> [A Good Explanation](https://doc.rust-lang.org/std/marker/trait.Copy.html#whats-the-difference-between-copy-and-clone) - The implementation of Clone can provide any type-specific behavior necessary to duplicate values safely. For example, the implementation of Clone for String needs to copy the pointed-to string buffer in the heap. A simple bitwise copy of String values would merely copy the pointer, leading to a double free down the line. For this reason, String is Clone but not Copy.
Note: Copy and Drop are mutually exclusive. Therefore, when a type dervies Drop, it cannot drive Copy [reference](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html#stack-only-data-copy). [More precisely, Copy means "it is safe to bitwise-copy the value and keep using the original value](https://stackoverflow.com/a/66770829). Last but not least, every move is bitwise-copy, so this is impportant for you to think about designing your code.

## Debug && Display
[Display](https://doc.rust-lang.org/std/fmt/trait.Display.html) is similar to [Debug](https://doc.rust-lang.org/std/fmt/trait.Debug.html) but `Debug` is for debugging usage. Besides, when a type derive `Display`, it automatically implement [ToString](https://doc.rust-lang.org/std/string/trait.ToString.html). When a type implements `fmt::Display`, it can faithfully be represented as UTF-8 [reference](https://doc.rust-lang.org/std/fmt/index.html#fmtdisplay-vs-fmtdebug). 
Lastly, `Display` is recommned to be implemented compared with `ToString`.

# In-Built fns

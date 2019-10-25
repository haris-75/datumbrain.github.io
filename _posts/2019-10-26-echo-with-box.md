---
layout: post
title: Echo with Box
author: Fahad Siddiqui
authorURL: https://github.com/fahadsiddiqui
date: "2019-10-26 01:22:00"
---

![](/assets/posts/echo-with-box/0.png)

I recently started refactoring one of my project's deployment scripts. I came across an enhancement where I wanted to make my strings boxed.

Like,
```
*********
* BOXED *
*********
```

Let's try doing this!

We need to create a function that can dynamically box any length of string passed to it. 

Let's start writing it 

```bash
echo_with_box () {
  # TODO 1: fetch length of argument $1 passed to this function
  # TODO 2: print *'s upto that length + 4 extra stars
  # TODO 3: print string *(space)$1(space)*
  # TODO 4: repeat TODO 2
}
```

## TODO 1

###  How can we find length of a variable string?

```bash
${#1} # gives you the length of $1
```

### How can we add four into it?

Why four? For first and third line in the BOXED example above you also have to take account of two asterisks and two spaces. That is why we have to put 4 extra stars in first and third line of this box.

```bash
$(expr ${#1} + 4)
```

## TODO 2

### Print upto `n` length

```bash
printf "%-2s" "*" # will output two asterisks eg; **
```

Let's pass our calculated length to this and store in a variable `pattern`

```bash
pattern=$(printf "%-${length}s"  "*")
```

### Print this pattern

```bash
echo "${pattern// /*}"
```

## TODO 2, 4: Wrapping all together in a function

```bash
echo_with_box() {
  length=$(expr ${#1} + 4) 
  pattern=$(printf "%-${length}s"  "*") 
  echo "${pattern// /*}" 
  echo "* $1 *" 
  echo "${pattern// /*}"
}
```

> Note: I could have done this using `boxes` utility available. But I wanted to do it the hard way. You might have to do it at some server where installing `boxes` is not an option available or you're simply not permitted to do so.

## Let's test this out
![](/assets/posts/echo-with-box/1.png)
![](/assets/posts/echo-with-box/2.png)
![](/assets/posts/echo-with-box/3.png)


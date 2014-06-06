---
layout: post
title: "The Logic Behind Logical Operators"
subtitle: "Using *&&*, *||*, *and*, and *or*"
date: 2014-02-13 18:26:23 -0400
comments: true
categories: 
---

One of the most compelling aspects of the Ruby language is the sweetness of its syntactic sugar. This made the almost ubiquitous use of the `&&` and `||` operators over their semantically pleasing counterparts and and or all the more surprising to me. It turns out the two sets of operators are actually different in a meaningful way.

### So what’s the difference?

**Precedence**. Huh? Precedence is the same as **Order of Operations**. Remember **PEMDAS** from grade school? It’s that acronym for **P**arentheses, **E**xponents, **M**ultiplication, **D**ivision, **A**ddition, **S**ubtraction. The operators at the beginning of **PEMDAS** (parentheses) are considered first, then the next (exponents), and so on. Operator precedence in Ruby and other programming languages works the same way — there are just more operators. Got it? Got it. Moving on…
<!--more-->

`&&` and `||` have a higher precedence than *and* and *or*. This means Ruby will consider them first. Seems kind of harmless as long as we’re not mixing the two sets of operators (e.g. using `&&` and `or`) in the same expression, but the real problem doesn’t lie at either of these levels of precedence. The troublesome area is the space between.

Assignment operators fall below `&&` and `||`, but above and and or. This starts to cause all sorts of problems when we use the latter because expressions stop behaving the way we assume they should.

```ruby
value = false || true  # (value) = (false || true)
  => true
value
  => true

value = false or true  # (value = false) or (true)
  => true
value
  => false
```

Let’s say we want to compare two values and assign their return value to a variable. The first set of operations occurs how we expect. The expression `false || true` returns `true`. Then `true` is assigned to `value`. The second set of operations looks like it should do the same, but it doesn’t. The `=` assignment operator has the highest precedence, so `false` is assigned to `value`. Then `value` is compared to `true` via the `or` operator. This returns `true`, but `value` returns false, which wasn’t our intention.

```ruby
value = true && false  # (value) = (true && false)
  => false
value
  => false

value = true and false  # (value = true) and (false)
  => false
value
  => true
```

We encounter the same problem with the code above.

There’s another key difference between these two sets of operators. `&&` is actually one level of precedence higher than `||`, whereas and and or share their level of precedence. I assume this is the case because of the assumption that we are more likely to compare chains of `&&` expressions with the `||` operator than vice versa because the former has more practical implementations. `and` and `or` have identical precedence, so their expressions are executed in the order in which they occur, which is rarely our intention.

### Cool, I get how it works. Now I can use and and or, right?

Wrong. Why would you do that to yourself? Because it’s prettier? Unless you’re writing specific types of expressions, you’ll have to resort to using unnecessary parentheses. Gross.

Because you just read this awesome blog post and now you know how they work? Well, did everyone you know read this blog post? I didn’t think so. Even if you know your code does one thing, your collaborators will probably assume it does something else.

### TL;DR

1. `&&` and `||` have a higher precedence than `and` and `or`.
2. Assignment operators, like =, have a lower precedence than `&&` and `||` (good), but higher than `and` and `or` (bad).
3. `&&` has higher precedence than `||` (good). `and` and `or` have the same (bad).
4. Using `&&` and `||` is the convention for a reason. Don’t be that guy.
5. If you get confused, just use parentheses. Parentheses are your friends. Make it work now. Refactor later.
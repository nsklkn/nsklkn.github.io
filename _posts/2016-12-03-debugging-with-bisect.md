Occasionally you can find a bug, but it is deeply rooted in history. In this case *git bisect* will help you.
Git bisect provides binary search through your commit history, revealing buggy one. In base case it looks something like that: you point git to start bisect process with `git bisect start`, indicate, that your commit has bug with `git bisect bad`, then you mark commit without bug with `git bisect good <commit SHA or tag>`. Looks simple.

Let's see how it look in real application. Here is my commit tree:

```
$ git log --pretty=oneline
603fc39d6c90bb4f20891788a0f9a1b20dba066c some changes #10
bc6bb9fc62db9718f586f9a33a71efc812100aab some changes #9
9a4ad0f04d3b7f96c5036708d7c20b4f0253ea06 some changes #8 <- error is here
a378b36488faa36cadbb119176e9eb9dfd3d9a7c some changes #7
e8e0009a6a4134563a23bf7a3027d8324b478cd8 some changes #6
2bfc65a1ebede991cc613c89bb5e839c3fb9714f some changes #5
48759346c5ddb71eae74b5fc6ac13a4a3ff29b4f some changes #4
b5fc84f9deac5e18f89d1f4c85d8a2bdd2b682e4 some changes #3
530507c83e910fa108234c607242235b2afefe96 some changes #2
d635cadfdc4093613d8a037ef0797663fb63d904 some changes #1
b60423b2edbfecf3234a7a0d91c393c59fe5c2e2 first commit
```

Here is my test application called *sum.rb* at firts commit:

```ruby
def sum(a, b)
  a + b
end

p sum(2, 2)
```

After some time of developing application, I made an error:

```ruby
def sum(a, b)
  a - b
end
```

So, I began debugging with *git bisect*.

```bash
$ git bisect start
$ git bisect bad
$ git bisect good b60423
Bisecting: 4 revisions left to test after this (roughly 2 steps)
[2bfc65a1ebede991cc613c89bb5e839c3fb9714f] some changes #5
```

Ok, we now at commit `2bfc65`, called some changes #5. Let's test our application:

```bash
$ ruby sum.rb
4
```

Seems like everything OK. Let's go deeper.

```bash
$ git bisect good
Bisecting: 2 revisions left to test after this (roughly 1 step)
[a378b36488faa36cadbb119176e9eb9dfd3d9a7c] some changes #
```

Our simple test says that even at this commit everything is OK, so let's continue debugging.

```bash
$ git bisect good
Bisecting: 0 revisions left to test after this (roughly 1 step)
[bc6bb9fc62db9718f586f9a33a71efc812100aab] some changes #9
$ ruby sum.rb
0
```

Whoa! Seems like we found the bug.

```bash
$ git bisect bad
Bisecting: 0 revisions left to test after this (roughly 0 steps)
[9a4ad0f04d3b7f96c5036708d7c20b4f0253ea06] some changes #8
$ git bisect bad
9a4ad0f04d3b7f96c5036708d7c20b4f0253ea06 is the first bad commit
```

Nailed it.

While debugging, I wrote `git bisect bad` instead of `git bisect good`. In this case you can reset bisect process with `git bisect reset`.

## Speeding up debug process

Ok, we found the bug, but it took some time. Even with 11 commits it takes us to check our script 4 times. But what should we do if our commit tree has not eleven, but thousand commits? Here is a solution: you can pass your test to bisect. Let's write one:

```ruby
require './sum'

sum(2, 2) == 4 ? exit(0) : exit(1)
```

```bash
$ git bisect start
$ git bisect bad
$ git bisect good b60423b2edbfecf3234a7a0d91c393c59fe5c2e2
...
9a4ad0f04d3b7f96c5036708d7c20b4f0253ea06 is the first bad commit
```

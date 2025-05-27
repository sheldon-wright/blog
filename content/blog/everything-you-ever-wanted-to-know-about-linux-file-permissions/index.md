---
title: "Everything You Ever Wanted to Know About Linux File Permissions"
slug: "everything-you-ever-wanted-to-know-about-linux-file-permissions"
date: 2025-05-26
draft: false
description: "Everything you need to know about Linux file permissions and then some."
topics: ["files", "Linux", "tutorials"]
---

# Everything You Ever Wanted to Know About Linux File Permissions

### What is a bit? 🔢

Let's start off simple. What is a bit?  A bit is a single unit of information. In binary, a bit can take the forms 1 or 0. 

A bit can represent a great many things, but typically `ON` or `OFF`. 

`0: OFF, 1: ON`

It can also represent larger numbers. For instance, with 3 bits we can represent 8 numbers.

`0 1 2 3 4 5 6 7`

The highest number is 7 and it occurs when all 3 bits are true (1)

```
111 from right to left

2^2     2^1     2^0
bit3    bit2    bit1 
1 - -   - 1 -   - - 1

2^2 (4) + 2^1 (2) + 2^0 (1) = 7
```

### Permissions 🔒

When it comes to permissions, there are 3 types of entities

`users, groups, others `

And 3 types of permissions

`read, write, execute`

Permissions can be represented in two different, but equivalent forms

1) Octal : number form 
2) Symbolic : letter form


#### Octal

Octal permissions are 3 digit numbers like 600. 

⚠ NOTE: Its very important to recognize that 600 is not 600 but actually 3 separate numbers.

```
user   group  others
- - -  - - -  - - -
  6      0      0
```

And each number represents the permissions that each entity has. 

```
r w x             base 2
-----             -------
r | |   read    = 2^2 (4)
  w |   write   = 2^1 (2)
    x   execute = 2^0 (1)

                        [r + w + x]
===================================
0 none                  [0 + 0 + 0] ---
1 execute only          [0 + 0 + 1] --x
2 write only            [0 + 2 + 0] -w-
3 write and execute     [0 + 2 + 1] -wx
4 read only             [4 + 0 + 0] r--
5 read and execute      [4 + 0 + 1] r-x
6 read and write        [4 + 2 + 0] rw-
7 read, write, and exec [4 + 2 + 1] rwx
===================================
```

`600` means

**owner** has `(6) read and write` because `4 + 2 + 0 = 6`

**group** has `(0) none` because `0 + 0 + 0 = 0`

**others** have `(0) none` because `0 + 0 + 0 = 0`


For good measure, how about some more examples!?

`644` means

**owner** has `(6) read and write` because `4 + 2 + 0 = 6`

**group** has `(4) read` because `4 + 0 + 0 = 0`

**others** have `(4) read` because `4 + 0 + 0 = 0`

The default permission for files is `666` and when a `umask` is applied it becomes `644`

What is a `umask` you might ask? A `umask` is a kind of policy that protects your system from 
assigning unsafe permissions by subtracting some permissions.

As mentioned previously, a `umask` of `022` would be subtracted from the default `666`

`666 - 022 = 644`

You wouldn't give everyone on the planet your house keys or the pin to unlock your phone would you? File permissions are no different.  

`755` means

**owner** has `(7) read, write, and execute` because `4 + 2 + 1 = 7`

**group** has `(5) read and execute` because `4 + 0 + 1 = 5`

**others** have `(5) read and execute` because `4 + 0 + 1 = 5`

Similarly, the default permissions for directories or folders is `777`. Applying our umask we get `755`

`777 - 022 = 755`
---
title: "Everything You Have Ever Wanted to Know About Linux File Permissions"
slug: "what-to-know-about-linux-file-permissions"
date: 2025-06-02
draft: false
description: "Everything you need to know about Linux file permissions."
summary: "Everything you need to know about Linux file permissions."
topics: ["Files", "Linux", "Tutorials"]

---

Let's start off simple...

## Bits

**What is a bit?** 

A bit is a single unit of information. 

In binary, a bit can take the form 1 or 0. 

`0: OFF` <br/>
`1: ON`

or if you prefer

`0: False` <br/>
`1: True`

We can even represent larger numbers with them. For instance, with 3 bits we can represent 8 numbers. 

This is known as an **octal**.

`0 1 2 3 4 5 6 7`

**So, how do we represent 8 numbers with only 3 bits?** 

Let's look at the largest number in the octal `7`. 

It occurs when all 3 bits are true `1`

`111` in expanded form looks like this

```
2^2     2^1     2^0
bit3    bit2    bit1 
1 - -   - 1 -   - - 1

(4) + (2) + (1) = 7
```

Let's try it again, and let's represent `4` this time.


```
2^2     2^1     2^0
bit3    bit2    bit1 
1 - -   - 0 -   - - 0

(4) + (0) + (0) = 4
```

See how it works?

Now that we've learned how binary and octal values fit together we will next learn how they relate to file permissions. 


## Permissions

When it comes to permissions there are

**3 types of entities**  `users, groups, others`

and 

**3 types of permissions**  `read, write, execute`

### Octal Notation

You can actually represent permissions in different ways, but for now we will focus on the octal form.

**Octal notation** is a numeric representation of permissions.

Octal permissions are 3 digit numbers like `600`. 

⚠ It's very important to recognize that `600` is **NOT** the number six-hundred. 

`6-0-0` is actually 3 separate and distinct numbers. Specifically, each number represents the permissions that each entity has.

```
user   group  others
- - -  - - -  - - -
  6      0      0
```
 

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

---

### Files

`644` is a common permission for files and it means

```
owner value is (6) read and write
because 4 + 2 + 0 = 6

group value is (4) read
because 4 + 0 + 0 = 0

others value is (4) read
because 4 + 0 + 0 = 0
```

**So, why 644?**

The default permission for files is `666` and when a `umask` is applied it becomes `644`

What is a `umask` you might ask? A `umask` is a kind of policy that protects your system from 
assigning unsafe permissions by subtracting some permissions.

For instance, a `umask` of `022` would be subtracted from the default `666`

`666 - 022 = 644`



### Directories


`755` is a common permission for directories and it means

```
owner value is (7) read, write, and execute 
because 4 + 2 + 1 = 7

group value is (5) read and execute 
because 4 + 0 + 1 = 5

others value is (5) read and execute 
because 4 + 0 + 1 = 5

```

**So, why 755?**

The default permissions for directories or folders is `777`. Applying our umask of `022` we get `755`

`777 - 022 = 755`


### Symbolic Notation

Now that we've covered octal notation let's discuss **symbolic notation**, the letter or symbol form of representing file permissions.

You might recall this from earlier 

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

It is actually the combination of *octal notation* and *symbolic notation*. 

Instead of numbers to represent permissions, we use letters (symbols)

`r` **read** is represented by the letter r

`w` **write** is represented by the letter w 

`x` **execute** is represented by the letter x

<br/>

And **entities** are represented symbolically also

`u` **user** or the file owner is represented by the letter u

`g` **group** is represented by the letter g 

`o` **others** are represented by the letter o

## `chmod` Command

The `chmod` command is how we actually use these notations in Linux to modify file permissions. 

`chmod` stands for **change-mode**.

## Special Permissions

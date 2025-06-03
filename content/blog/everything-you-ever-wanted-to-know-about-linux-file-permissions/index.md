---
title: "Everything You've Ever Wanted to Know About Linux File Permissions"
slug: "what-to-know-about-linux-file-permissions"
date: 2025-06-02
draft: false
description: "Everything you need to know about Linux file permissions."
summary: "Everything you need to know about Linux file permissions."
topics: ["Files", "Linux", "Tutorials"]

---

Let's start off simple

**What is a bit?** 

A bit is a single unit of information. 

In binary, a bit can take the form 1 or 0. 

`0: OFF` <br/>
`1: ON`

or if you prefer

`0: False` <br/>
`1: True`

We can even represent larger numbers with them. For instance, with 3 bits we can represent eight distinct values which correspond to the eight octal digits

`0 1 2 3 4 5 6 7`

**So, how do we represent 8 numbers with only 3 bits?** 

| Binary | Octal |
| ------ | ----- |
| 000    | 0     |
| 001    | 1     |
| 010    | 2     |
| 011    | 3     |
| 100    | 4     |
| 101    | 5     |
| 110    | 6     |
| 111    | 7     |


Let's look at the largest octal digit `7`. 

It occurs when all 3 bits are true `1`

`111` in expanded form looks like this

```
2^2     2^1     2^0
bit2    bit1    bit0 
1 - -   - 1 -   - - 1

(4) + (2) + (1) = 7
```

Let's try it again, and let's represent `4` this time.


```
2^2     2^1     2^0
bit2    bit1    bit0 
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
because 4 + 0 + 0 = 4

others value is (4) read
because 4 + 0 + 0 = 4
```

**So, why 644?**

The default permission for files is `666` and when a `umask` or creation mask is applied it becomes `644`

What is a umask you might ask? A `umask` is a kind of policy that protects your system from 
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

`a` **all** is represented by the letter a

## `chmod` Command

Before we change the permissions, let's quickly go over how to view the current permissions by running the command `ls -l` to get a long listing of the current working directory. 

This command should output something like

```
drwxr-xr-x  8 user group 4096 Jan 12 09:00 Desktop
drwxr-xr-x 10 user group 4096 Jan 12 09:00 Documents
drwxr-xr-x  6 user group 4096 Jan 12 09:00 Downloads
drwxr-xr-x 12 user group 4096 Jan 12 09:00 Music
drwxr-xr-x  9 user group 4096 Jan 12 09:00 Pictures
drwxr-xr-x  5 user group 4096 Jan 12 09:00 Videos
-rw-r--r--  1 user group  220 Jan 12 09:00 somefile
```

The `chmod` command is how we actually use these notations in Linux to modify file permissions. 

`chmod` stands for **change-mode**.

The general format for the command using **octal notation** is 

`chmod [octal value] [filename] `

e.g. 

`chmod 644 filename`

This version explicitly sets permissions for all three entities at once. 

The general format for the command using **symbolic notation** is 

`chmod [entity][operator][permission] [filename] `

Suppose you wanted to add read permissions to the group, you would run the command

`chmod g+r filename`

If you wanted to remove it you would run the command 

`chmod g-r filename`

If you wanted to add read permission to all of the entities you could run

`chmod a+r filename`

To make a file executable you can omit the entity entirely using the command 

`chmod +x filename`

## Special Permissions

Thus far I've only mentioned 3 pieces of information related to permissions, but there's actually 4. 

The `umask` we spoke about earlier assumes a special permission of 0 or no special permissions. 

`022` is equivalent to `0022` where the left most digit represents the special permissions 

This table displays the possible values for special permissions

| Permission      | Binary | Octal |
| --------------- | ------ | ----- |
| None            | 000    | 0     |
| Sticky          | 001    | 1     |
| SGID            | 010    | 2     |
| SGID + Sticky   | 011    | 3     |
| SUID            | 100    | 4     |
| SUID + Sticky   | 101    | 5     |
| SUID + SGID     | 110    | 6     |
| All             | 111    | 7     |



**SUID** stands for Set User ID. You set SUID on the owner level. It runs an executable file with file owner’s permissions 

**Symbolic**: `chmod u+s filename`

**Octal**: `chmod 4XXX filename`

<br/>

**SGID** stands for Set Group ID. You set SGID on the group level. It makes any new file in that directory inherit the directory’s group, and (when set on an executable) it runs with the file’s group permissions.

**Symbolic**: `chmod g+s filename`

**Octal**: `chmod 2XXX filename`

<br/>

**Sticky Bit** is set on the world or others level on a directory. It allows sharing of files in the directory, but only a file’s owner may delete a file.

**Symbolic**: `chmod o+t directoryname`

**Octal**: `chmod 1XXX directoryname`
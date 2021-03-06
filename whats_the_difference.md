# <a name="whats-the-difference"></a>What's the difference

**Table of Contents**

* [cmp](#cmp)
* [diff](#diff)

<br>

## <a name="cmp"></a>cmp

```bash
$ cmp --version | head -n1
cmp (GNU diffutils) 3.3

$ man cmp
CMP(1)                           User Commands                          CMP(1)

NAME
       cmp - compare two files byte by byte

SYNOPSIS
       cmp [OPTION]... FILE1 [FILE2 [SKIP1 [SKIP2]]]

DESCRIPTION
       Compare two files byte by byte.

       The optional SKIP1 and SKIP2 specify the number of bytes to skip at the
       beginning of each file (zero by default).
...
```

* As the comparison is byte by byte, it doesn't matter if file is human readable or not
* A typical use case is to check if two executables are same or not

```bash
$ echo 'foo 123' > f1; echo 'food 123' > f2
$ cmp f1 f2
f1 f2 differ: byte 4, line 1

$ # print differing bytes
$ cmp -b f1 f2
f1 f2 differ: byte 4, line 1 is  40   144 d

$ # skip given bytes from each file
$ # if only one number is given, it is used for both inputs
$ cmp -i 3:4 f1 f2
$ echo $?
0

$ # compare only given number of bytes from start of inputs
$ cmp -n 3 f1 f2
$ echo $?
0

$ # suppress output
$ cmp -s f1 f2
$ echo $?
1
```

* Comparison stops immediately at the first difference found
* If verbose option `-l` is used, comparison would stop at whichever input reaches end of file first

```bash
$ # first column is byte number
$ # second/third column is respective octal value of differing bytes
$ cmp -l f1 f2
4  40 144
5  61  40
6  62  61
7  63  62
8  12  63
cmp: EOF on f1
```

**Further Reading**

* `man cmp` and `info cmp` for more options and detailed documentation


<br>

## <a name="diff"></a>diff

```bash
$ diff --version | head -n1
diff (GNU diffutils) 3.3

$ man diff
DIFF(1)                          User Commands                         DIFF(1)

NAME
       diff - compare files line by line

SYNOPSIS
       diff [OPTION]... FILES

DESCRIPTION
       Compare FILES line by line.
...
```

* `diff` output shows lines from first file input starting with `<`
* lines from second file input starts with `>`
* between the two file contents, `---` is used as separator
* each difference is prefixed by a command that indicates the differences (see links at end of section for more details)

```bash
$ paste d1 d2
1       1
2       hello
3       3
world   4

$ diff d1 d2
2c2
< 2
---
> hello
4c4
< world
---
> 4

$ diff <(seq 4) <(seq 5)
4a5
> 5
```

* use `-i` option to ignore case

```bash
$ echo 'Hello World!' > i1
$ echo 'hello world!' > i2

$ diff i1 i2
1c1
< Hello World!
---
> hello world!

$ diff -i i1 i2
$ echo $?
0
```

* ignoring difference in white spaces

```bash
$ diff <(echo 'good day') <(echo 'good    day')
1c1
< good day
---
> good    day
$ # -b option to ignore changes in the amount of white space
$ diff -b <(echo 'good day') <(echo 'good    day')
$ echo $?
0

$ diff <(echo 'hi    there ') <(echo ' hi there')
1c1
< hi    there 
---
>  hi there
$ # -w option to ignore all white spaces
$ diff -w <(echo 'hi    there ') <(echo ' hi there')
$ echo $?
0
$ diff -w <(echo 'hi    there ') <(echo 'hithere')
$ echo $?
0
```

*More to come*

<br>

**Further Reading**

* `man diff` and `info diff` for more options and detailed documentation
* `diff3`, `vimdiff/gvimdiff` and `patch` commands
* [diff Q&A on unix stackexchange](https://unix.stackexchange.com/questions/tagged/diff?sort=votes&pageSize=15)
* [GUI diff and merge tools](http://askubuntu.com/questions/2946/what-are-some-good-gui-diff-and-merge-applications-available-for-ubuntu)
* [unix.stackexchange - Understanding diff output](https://unix.stackexchange.com/questions/81998/understanding-of-diff-output)
* [stackoverflow - Using output of diff to create patch](https://stackoverflow.com/questions/437219/using-the-output-of-diff-to-create-the-patch)


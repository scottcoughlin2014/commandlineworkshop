# 1- Navigate Files and Directories

### Questions
- How can I move around on my computer?
- How can I see what files and directories I have?
- How can I specify the location of a file or directory on my computer?

## Navigate on your file system

```bash
# the command line only understands meaningful instructions
# speak the computer language
```

```bash
pwd # explain what a path is

ls 

```

```bash
cd abs_path/data-shell
pwd
# introduce tab
```

```bash
cd /absolute/path/to/data-shell
cd relative_path/data-shell
```


```bash
cd        # move to home folder
cd .      # "." means same folder. You stay in the same place.
cd ..     # ".." means go one level up
cd ../..  # "../.." means go two level up
cd ~      # "~" is a shortcut for home folder.
cd /      # "/" means go to root (upper most) level
cd -      # "-" means go to the previous directory
```

```bash
cd /absolute/path/to/data-shell
ls
cd data-shell
ls
ls molecules
```

```bash
ls -l # explain each column
ls -hl
ls -h -l
```

```bash
<command> --help

man <command>
```

# 2- Working with Files and Directories

### Questions
- How can I create, copy and delete files and directories?
- How can I edit files?


```bash
pwd 
cd /absolute/path/to/data-shell
ls -hl
nano notes.txt
```

`cp` (copy) is used to create copies of files or folders.

```bash
# make a copy of notes.txt
cp notes.txt notes-copy.txt
# copy a directory
cp -r cretures creatures-copy # -r means recursive
# copy a file into a directory
cp notes-copy.txt creatures-copy
ls -lh creatures-copy
```

When you copy a folder you also copy its contents. If you want to create
a new empty folder, you can use `mkdir` (make directory).

```bash
mkdir creatures-empty
ls -lh
ls -lh creatures-empty
```

"notes-copy.txt" file, "creatures-copy" and "creatures-empty" directories
seem redundant. Let's delete them. We will use `rm` (remove) command for
deletion. **Warning**: Be very careful when using `rm` as the deletion
process will be **irreversible**.

```bash
rm notes-copy.txt
# BE VERY CAREFUL WITH THIS
rm -r creatures-copy creatures-empty 
```


Instead of copying, you can use `mv` (move) to displace files and folders.

```bash
pwd
ls -hl
cd molecules
ls -hl # now we are inside molecules dir
mv ../creatures . # .. and . are not only for `cd`
# creatures folder moved here
ls -hl
cd ..
ls -hl # creatures folder gone
mv molecules/creatures .
ls -hl # creatures folder back
```


We can also create empty files with `touch` command.
```bash
touch my-file.txt
ls -lh
```

```bash
nano my-file.txt
```
Type your name into this file, save (hit <kbd>CRTL</kbd>+<kbd>o</kbd>
then hit <kbd>Enter</kbd>/<kbd>Return</kbd>) and exit (hit
<kbd>CTRL</kbd>+<kbd>x</kbd>).




# 3- Pipes and Filters

### Questions
- How can I do more with less typing? Any shortcuts?
- How can I combine existing commands to do new things?


### I. Wildcards

```bash
cd molecules
ls -hl
```

```bash
wc cubane.pdb
```

```bash
wc --help
wc -l cubane.pdb # number of lines
wc -w cubane.pdb # number of words
wc -m cubane.pdb # number of chars
wc -c cubane.pdb # number of bytes # ASCII character takes 8 bits (1 byte)
```

Instead of issuing the command for each file one by one, or
using a loop to go though each file we can use **wildcards**. 

```bash
wc *.pdb # all pdb files
wc p*.pdb # all pdb files that start with a 'p'
```

```bash
# ? only expands into ONE char while * could be 0-infty
wc p?tane.pdb
wc p??tane.pdb
wc p*tane.pdb
```

```bash
mkdir temp_dir
cp p*.pdb temp_dir
ls -hl temp_dir
rm -r temp_dir
```

### II. Sorting and Redirecting
Let's gather information about the number of lines in all files in this folder and 
store the data

```bash
pwd
wc -l *.pdb
wc -l *.pdb > lengths.txt
```

Did we create a file?

```bash
ls lengths.txt
ls -lh lenghts.txt
```

What is in the "lengths.txt" file? We can use `cat` (concatenate) command to
print the contents of the file. `cat` prints out all the file to screen but
if we want to see the data page by page, we can use `less`. You can move screen
by screen with spacebar, or go back by `b` and quit by `q`

```bash
cat lengths.txt # don't use `cat` for large txt files
less lengths.txt # hit `q` to exit
cat # the process started waiting for some input from you, use Ctrl+C to exit
```

One other type of redirecting is appending with `>>`

```bash
wc -l *.pdb >> lengths.txt # `>` is overwrite, `>>` is append
cat lenghts.txt
rm lenghts.txt
wc -l *.pdb > lenghts.txt
```

We want to see the which molecule file has the most lines.
We can use `sort` command.

```bash
sort -n -k 1 lengths.txt
sort -k 2 sorted-lenghts.txt
```

In sort "-n" identifies numeric sorting. If not set, the default is to
sort alphabetical. "-k" is used to define the column. If  "-k" is not
set, the default is the first column.

Let's redirect the result to another file and look at the shortest and
longest files. 

```bash
sort -n lengths.txt > sorted-lenghts.txt
head -n 1 sorted-lenghts.txt
tail -n 1 sorted-lenghts.txt
```

`head` prints out a number of lines from the top of the file whereas
`tail` prints out a number of lines from the bottom.

Redirecting a file to the same file is a bad practice, don't do
`sort -n lengths.txt > lenghts.txt`

We can reverse the order of `sort` or the column that the sorting
is done.

```bash
sort -n -r lengths.txt
```

### III. Pipes

We have used `wc`, `head`, `sort` commands step by step to accomplish tasks.
There is another approach called **Pipes** with which we can execute our 
tasks in one step.

```bash
sort -n lengths.txt | head -n 1
sort -n -r lengths.txt | tail -n 1
```

The vertical bar `|` between to commands is called a **pipe**. It takes the 
output from the command on the left and feeds as input to the command on the
right. 

```bash
sort -n lengths.txt | head -n 3 | tail -n 1
wc -l *.pdb | sort -n | head -n 3 | tail -n 1
```

As you see, we can create a pipeline by stacking many pipes one after 
another. Each time the data is filtered and modified in some manner.

Now we will look at 2 other useful command we can use when filtering data.
These are `uniq` and `cut`

```bash
cp ../data/salmon.txt ./
cat salmon.txt
uniq salmon.txt; cat salmon.txt | uniq ; uniq < salmon.txt
sort salmon.txt | uniq
```

`uniq` removes duplicate lines only if they are adjacent. Combining with `sort`
you can eliminate all duplicates.


# 4- Finding Things

### Questions
- How can I find files?
- How can I find things in files?

To find things in your system, the two commands that
you will most use are `grep` (global/regular expression/print) 
and `find`.

`grep` finds and prints lines in files that match a pattern.

Let's move to 'writing' folder

```bash
cd ../writing
pwd
ls -lh
```

What is in haiku.txt?

```bash
cat haiku.txt
```

Let's find lines that contain the word "not" and "The" :

```bash
grep not haiku.txt
grep The haiku.txt
```

When we searched for "The" two lines came up. In one the 
letters are actually included in "Thesis". To restrict our
search to the word "The" we could use `-w` flag. This flag
forces the pattern to match only whole words.

```bash
grep -w The haiku.txt
```

What is the line number of the line with matching the pattern? 
`-n` flag prints out the line number

```bash
grep -n -w The haiku.txt
grep -n The haiku.txt
```

How do we find all "The" and "the" together? `-i` makes the search
case-insensitive

```bash
grep -n -w the haiku.txt
grep -n -w -i The haiku.txt
```

How do we find all lines that do not contain "the" or
"The"? `-v` flag inverts the selection

```bash
grep -v -n -w -i The haiku.txt
``` 

There are other flags you can explore, just type:

```bash
grep --help
``` 

We could also search for patterns including spaces.

```bash
grep "is not" haiku.txt
```

We searched within files using `grep`. `find` command 
on the other searches for files. Let's see what `find`
finds in the current folder

```bash
find . # '.' means current directory
find ../ # '../' means one directoy above
```
`find`s output is the names of every file and directory 
under the current working directory. Using `-type d` and
`-type f` flag/argument couples we can determine directories
or files respectively 

```bash
find . -type d
find . -type f
```

`-name <pattern>` flag can be used to find files with
matching pattern in their name

```bash
find . -name *.txt
```

We expected it to find all the text files, but it 
only prints out "haiku.txt". The problem is that 
the shell expands wildcard characters like * before 
commands run. Since *.txt in the current directory 
expands to "haiku.txt", the command we actually ran was:

```bash
find . -name haiku.txt
```

To find all '.txt' files:

```bash
find . -name '*.txt'
```

Putting *.txt in single quotes to prevent the shell from 
expanding the * wildcard. This way, find actually gets 
the pattern *.txt, not the expanded filename haiku.txt

Can we find out the number of lines for all '.txt' files

```bash
wc -l $(find . -name '*.txt')

#OR

wc -l `find . -name '*.txt'`
```

Here the commands in '$()' and '``' are evaluated and 
then `wc -l` operates the output. We can further
`sort` the result

```bash
wc -l `find . -name '*.txt'` | sort -n
```

We can also use `find` and `grep` in sequence to search
for a pattern in certain files
 
```bash
grep -w 'FE' $(find .. -name '*.pdb')
```


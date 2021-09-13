# Lesson1_Unix

Repo adapted from 2019 Environmental Bioinformatics prepared by @halexand

### Review
**Last time we: **
1. Opened the terminal on our local machine
2. Talked about file / folder structures
3. Tested out some  commands for navigating around our file structures via the command line and figuring out who we are:
        - `ls`: list files in current location
        - `pwd`: print working directory (where am I?)
        - `cd`: change directory (moving from place to place)
        - `unzip`: unzip a file compressed with zip function

### Goals for today: 
**Today we will:**
1. Get an introduction to HPCs in general and the HPC we will be using for this class (`poseidon.whoi.edu`) 
2. Discuss remote machine access (`ssh`) and log on to the HPC
3. Review the commands we learned at the end of last class 
4. Review wildcards and regex
5. Review outputs & string commands together
6. Review scripts


## Logging on to the HPC
For this class (and all subsequent classes) we will be using WHOI's HPC: `poseidon.whoi.edu`. This is a remote cluster of computers (it isn't really *that* remote.. it is in the basement of Clark). Nearly all command line systems (BASH etc.) have Secure Shell (`ssh`) natively installed. `ssh` is a cryptographic network protocol that allows you to provides a secure channel over an unsecured network. `ssh` can be used to log on to any number of platforms such as: remote computers (like a lab computer), computer clusters or high performance computers (HPCs), computers running in the cloud (e.g. AWS), etc. 

Today we will be using `ssh` to logon to `poseidon.whoi.edu`. To do this you will need to know what your WHOI username is and be within WHOI's firewall either by connecting to the local network (e.g. `Arctic`) or by logging into WHOI's vpn. 

Once you are logged on to WHOI's network you should be ready to `ssh`. In your terminal prompt type the following: 

```bash
ssh username@poseidon.whoi.edu
``` 
If this is your first time logging onto the network you will see a prompt like the following: 

``` text 
	Host key not found from the list of known hosts.
	Are you sure you want to continue connecting (yes/no)? 
```
This is a safety protocol ensuring that you do indeed want to add this address to a list of known hosts. Type `yes`. 

You should now be prompted for your password (this will be your WHOI email password): 
``` text 
Host 'poseidon.whoi.edu' added to the list of known hosts.
      [usernames]'s password:
```
Once you type the correct password you should see something like the following:
```text
  ___             _    _             ___ _         _           
 | _ \___ ___ ___(_)__| |___ _ _    / __| |_  _ __| |_ ___ _ _ 
 |  _/ _ (_-</ -_) / _` / _ \ ' \  | (__| | || (_-<  _/ -_) '_|
 |_| \___/__/\___|_\__,_\___/_||_|  \___|_|\_,_/__/\__\___|_|  

Welcome to the Poseidon Cluster at WHOI!

Please remember to copy your files to scratch and move/delete them after each job.
Please do not run anything on the login nodes and submit jobs to SLURM. All running 
jobs/processes on the login nodes will be terminated without notice.
```
Congratulations! You have logged on to the HPC! This terminal now represents the environment of the remote computer that you just logged on to. 


>Look at your `prompt`. Has it changed? What information do you see now? What do the different parts mean? Hint: try using the command `whoami` and the command `hostname`. 

### Navigating to our classroom directory
For this class we have set up a special workspace on `poseidon`. This is where you will do all your homework and projects. 

>Navigate to `/vortexfs1/omics/env-bio`. What folders do you see?  Hint: if you are typing out the path provided rather than copying it, try hitting the `tab` key in the middle of the word or phrase, this should automatically complete the phrase for you *or* if there is more than one option repeatedly hitting `tab` will bring up a listing of all options. 

You should see a folder within `/vortexfs1/omics/env-bio/` called `users/`. This directory holds many subdirectories that we will be using in the class. 
>Run `ls users`. What do you see? 

Now, navigate into your user directory. This will be your computational space for the rest of the class. Only you  and the instructors have access to your directory. 
>Try navigating into someone else's directory. What happens? Why? Try `ls -l` to figure it out.

This is a nice example of how [file permissions](http://linuxcommand.org/lc3_lts0090.php) can be used to structure access to a file system. The letters you see next to the file indicate the permissions (read, write, execute) that are given to different groups from left to right (owner, group, all). You can **actively** use file permissions to protect original data (we will come back to this later). We will also learn how to change those permissions later.

### Getting ready!
Now, if for some reason you have moved away, navigate back to your class user directory. Use `ls` to check the contents of the folder. Are there any files or folders?

Get the class material from https://github.com/environmental-bioinformatics-master/unix-folders/archive/master.zip


And unzip it. Unlike last class where you had the option of unzipping `master.zip` through your computer's GUI file navigator (`Finder` or the like), on the HPC you DO NOT have access to any sort of GUI and you must do everything via the `CLI`. 

Now, navigate into the newly created folder `unix-folders-master/`. 

>How can you tell if a listing is a folder? 

### Working with files and directories
Run `ls -F`. How many directories are there in this level? 



#### Creating directories
Now, let's navigate back to `unix-folders-master/`.  In this directory we are going to make a new directory called `observations`. Which command should we use?

>Run `ls -F` again. What has changed since the first time? Try running `ls -F observations`. What do you see? 

Since we just made `observations` there is nothing inside of it-- let's change that! 

#### Creating a file
Move into the directory `observations` and let's create a file. There are (unsurprisingly) many ways to do this. For now, let's use a text editor. During this class we will be using `nano` as it one of the easiest to learn. There are many other more powerful command line text editors (e.g. `vim`, `emacs`, etc.). If you are comfortable with one of those feel free to use it-- otherwise stick with nano. 

To create a new text file simply type `nano [name of file]`, for example:
```bash
nano 2021-observation.txt
```
 This will create a new file called `2021-observation.txt` and you should be automatically be entered into the nano environment. Add some text to the file. And then press `ctrl+O`. This will write the text to the output file. Once you are satisfied you can press `ctrl+X` to exit. Note that the bottom of the nano window lists other `ctrl` commands that you might want to use with the program. 

> We just made a file with nano. Now investigate the command `touch`. Try typing `touch 2021-observation.txt`. What happened? How is it different from nano? Try looking at its size. 

>> **Question for thought:** If you make a text file does it need to end in `.txt`?

#### Looking at files (without editing them)
Often we want to get a quick look at the contents of a file without potentially accidentally modifying it. For that, the command `less` is quite useful. Try typing `less 2021-observation.txt`. This should bring you into an interface where you can read and scroll but cannot edit the document. Type `q` to exit. Take a look at both files in this directory. 

Another convenient way to examine the content of a file is to print it to the screen or print it to `stdout` or Standard Output. The program `cat` or concatenate will print the entire contents of a file to the screen. Try it out by typing cat `2021-observations.txt`. 
>How is this different than less? 

#### Moving and renaming
One of the most common things you will likely find yourself doing is managing and reorganizing files in your projects. Let's go to the folder called `unix-folders-master/writing/drafts/`. Here we have many different versions of a paper that we are struggling to write. 

First, let's make a new directory called `old-drafts`. We are going to use the command `mv` (move) to move files on the command line. `mv` takes two arguments: the first is the target file that you want to move, the second is destination or the location that you want to move it to. Let's move `paper-v1.txt` into `old-drafts/`. 

The command `mv` can also be used to rename files. Let's rename `paper-final2.txt` to `paper-DONE.txt`. To do this, your destination is the new name of the file. 
```bash
mv paper-final2.txt paper-DONE.txt
```
>Run `ls`. What has happened? 

`mv` can also be used with directories. For example, you can change the name of the directory `old-drafts` to `extremely-old-drafts`. 

**A word of caution: ** You can *very* easily overwrite files using `mv`. Let's check this out. What happens if you run:
```bash
mv paper-v4.txt paper-v3.txt
```
Do you have the same number of files? 

> Write a single command to move `paper-v2.txt` to the directory `unix-folders-master/observations/` *and* rename it `initial-observations`.  Now, navigate to `unix-folders-master/observations/` and try copying `paper-v3.txt` to the location where you are without moving. 

#### Copying
The command `cp` (copy) works very similarly to `mv`. As with `mv` it requires two commands (the target file you want to copy and the destination/name of the new file). Return to `unix-folders-master/writing/drafts/`. Let's try to create the files that we had previously by copying our final drafts. 

```bash
cp paper-final.txt paper-v1.txt
cp paper-final.txt paper-v2.txt
cp paper-final.txt paper-v3.txt
```
> `cp` can also be used with directories. Try copying `extremely-old-drafts` to a new directory called `very-old-drafts`. What happened? Use `man` to figure out if there is a flag that can help you. 

#### Removing things
**On the command line... removal is permanent.**
Removing with the command `rm` just as `cp` and `mv` can take a target file or a list of more than file. So, for example `rm paper-v1.txt` removes `paper-v1.txt`. If you run `ls` again you will note that it is gone. And it is *truly gone*. Short of having backed things up-- there is *no way* to get a `rm`-ed file back. A flag that I like to add (or have aliased) is `rm -i` for interactive. Let's try `rm -i` to remove `paper-v2.txt`. 

**The danger zone:** You can also use `rm` to remove directories and this is where you *really* need to be careful. Using the flag `rm -r` will recursively remove all files within a directory. I strongly suggest (especially if you are new the the command line) using the `-i` flag. Note: `rmdir` is another option for removing directories -- the default requires that the directory be empty. 


## Wildcards and Regular Expressions
### Wildcards
Wildcards, such as `*` are used to identify multiple files or folders that match a requested pattern. `*` matches zero or more characters and is often used to grab groups of files. Navigate to the folder `unix-folders-master/data/` and type `ls`. If we wanted to see what files ended in `.out`, we can use the wildcard `*`. For example, `ls *.out` will return any file name that ends in the pattern `.out`. 

> Use `*` to list all files that end in `.sh`. 

A note on usage: When the shell sees a wildcard, it expands the wildcard to create a list of matching filenames before running the command. If a wildcard expression does not match any file, Bash will pass the expression as an argument to the command as it is. For example typing `ls *.pdf` here will result in an error message that there is no file called `*.pdf`.  Ultimately, it is the shell, not the other programs, that deals with expanding wildcards, and this is another example of orthogonal design.
 
 Navigate  to `unix-folders-master/measurements/`. This folder contains a bunch of different measurement files. Use `less` to take a look at one of them. 

> Use `*` to print all the measurements that *end in* the number 5.
> Use `*` to print all the measurements that *contain* the number 5. 
> Use `*` to print all the measurements whose number *starts with* the number 5. 
#### Regular expressions

Regular expressions (abbreviated regex) are a concept used in many different programming languages for sophisticated pattern matching. When used well, they can be a very powerful tool to help you find and transform your data and files. 

A regular expression is a sequence of characters used to define a search to match strings (think find and replace in Word). A ‘string’ is a contiguous sequence of symbols or values. For example, a word, a date, a set of numbers (e.g., a phone number), or an alphanumeric value (e.g., an identifier). The wildcard `*` we just learned about is taken from regular expressions, but there are many more features to the complete regular expressions syntax:
- Match on types of characters (e.g. upper case letters, digits, spaces, etc.).
- Match patterns that repeat any number of times
- Capture the parts of the original string that match your pattern

Regular expressions look at both the actual charter and the  literal characters and `metacharacter`. A metacharacter is any American Standard Code for Information Interchange (ASCII) character that has a special meaning. Using these together, you can construct a regex for finding strings or files that match a pattern rather than a specific string. 

##### Common regex metacharacters
Square brackets  define a list or range of characters to be found: 
- `[ABC]` matches A or B or C.
- `[A-Z]` matches any upper case letter.
-  `[A-Za-z]` matches any upper or lower case letter.
- `[A-Za-z0-9]` matches any upper or lower case letter or any digit.
And then some special commands are : 
- `.` matches any character
- `\d` matches digits
- `\w` matches any word character (i.e. not spaces)
- `\s` matches white space (space, tab new line etc.)
- `\` is a general 'escape character'
And then there are positional commands:
- `^` means that the position must be at the start of the line
- `$` means it must appear at the end of the line

> So, what is `^[Aa]naly.e` going to match? 

Now, let's use it like we were using the wildcard `*`.

> **Exercises**
> 
> Write a command that uses regex to list all files that contain a 5 or a 6 somewhere within their name. 
> 
> Write a command that lists all files that lists all files that end in 3, 7 or 8. 
> 
> Write a command that returns all files that contain a 2 followed by a 1 or an 8. 

These are just the basics. If you are wanting to learn more about regex I recommend checking out: 

- https://regexone.com/ : a great interactive online learning tool. 
- https://regexr.com/ : a useful regex tester 
- https://regexcrossword.com/ : for the regex fans who have too much time on their hands. 

### Outputs & string commands things together 
Alright, so we can now navigate around file structures with the command line, make files and directories, copy things, remove things, rename things. Great! These are all things we could arguably do just as easily with your GUI interfaces. The real power of shell comes from our ability to string commands together to do something greater. 

Navigate into the directory `data`. Type `ls` to familiarize yourself with what we are doing. Again let's use `ls` to look at the files. There are many data files that end in `.out`. Take a look at one of with `less`. 

We are going to use a new command now called `wc` which counts the lines, words, and characters in a file. 

Run `wc *out`. This is printing information to standard out `stdout`. This is the default output from many programs-- it prints an answer for you on the command line. This answer, however, is not saved anywhere. Within command line we can actively redirect outputs to save it into a file or to pass the output into a new function. Let's check out what that looks like. 


![Alt text](./1568056934210.png)
Image from [Software Carpentry](https://swcarpentry.github.io/shell-novice/04-pipefilter/index.html). 

Let's run `wc -l *out` (this only counts the lines for each of the files. We can now use the `>` to pass this output to a file. Let's make a file called `lengths`. 

```bash
wc -l *out > lengths
```
> What happened? Was anything printed to standard out? 

>>As an aside: `>` writes to a new file. It will overwrite anything present within a file. `>>` by contrast can be used to *append* to an existing file by adding the output to the end of an existing file. 

What is more is you can actually pass out put to other programs. Let's introduce briefly a couple very useful programs for looking at and dealing with files:

- `wc` : counts the words in the file
- `sort`: sort the contents of a file
- `uniq`: returns only unique values from a file. Requires the file has been sorted
- `head`: returns the head (top) of the file
- `tail` : returns the tail (bottom) of the file
- `cut`: cuts a column of interest based on whatever the delimiter is
- `paste`: pastes columns together 
- `cat`: in addition to printing a file to screen it can be used to concatenate files together
- Any other class favorites? 

These are some of my most used text handling functions that I use. All of them default to outputting to `stdout` and so can be easily piped together. [Note: `sed`, `grep`, and `find` are still to come-- they deserve their own special attention.]

So, let's try piping the output of `wc -l` into a new function. Let's try sort so that we can figure out what file has the most lines. To pass the output of one file to the next we use the pipe character`|` (located above the `\` on American keyboards). So, we will pass the output of `wc -l` to `sort -n` to get a list of the files sorted based on the numeric value. 

```bash
wc -l *out|sort -n
```
Now we are looking at a sorted list. We can then programmatically identify the shortest command by piping the output of the above to another command `head -n 1` which will return only the first line of the input. 

```bash
wc -l *out|sort -n|head -n 1
```
We can then even save this output to a file! 

```bash
wc -l *out|sort -n|head -n 1 > shortest.file
```
You can see how these tools can be endlessly pieced together to do some really powerful manipulations!

> **Exercise Break:** Find a partner and try working through  these exercises.
>- What is the difference in function between `sort` and `sort -n`. Try sorting one of the `.out` files to figure it out. 
>- Write a command that will identify the index value ( number in the first column) that has the largest value in the third column of `hiztory.out` . Return *only* the number in the first column and save the input to a file called big index. Hint: look at the man pages to find useful flags. 


### Automation with for loops
**Loops** are a programming construct which allows us to perform a series of commands in the same way for each item in a list. Loops facilitate automation and save you time-- moreover loops reduce the amount of typing required (and mistakes made). So, if you ever find yourself thinking: gosh-- I want  to do this one thing to all 10502395 of my files. You should think: `for loop`.

Loops are common across programming languages. Today we will be  writing one in `bash` but we will also learn how to write them in `python` later on. Regardless of the language the general format is the same:

```
### PSEUDO CODE
FOR item in LIST
	DO_SOME_STUFF
DONE
```
In `bash` for loops look like this:

``` bash
for item in list
do
	DO_SOME_STUFF $item
done
```
When the shell sees the word `for` it knows that a loop is coming. It 

The `$` in `bash` is used to designate variables. We have already seen variables (e.g. `$HOME` and `$PATH`).  A variable name is a name whose value can be changed (rather than a text string or command that is static and cannot be changed. In a for loop the `item` in a list becomes a variable and changes as it moves through the loop. 

Navigate to `data/`. Let's pretend we wanted to retrieve the 25th value from `lion.out` and `secret.out` data files. If we tried to do this with our wildcard command and pipes it wouldn't work very well. We can encode this in a for loop. 

```bash	
for file in lion.out secret.out
do
	head -n 25 $file | tail -n1
done 
```
> What if we wanted to save this out to a file? Modify this to create a file that has the output. This can be done in more than one way. 

Now let's say that we want actually copy each of our `*out` files.  If we tried to do this with our wildcard command and pipes it wouldn't work very well. We can encode this in a for loop. 

> Note: The shell prompt changes to  `>`  as we were typing in our loop. This indicates that the terminal is waiting for something to tell it that the command is finished. In this case `done`. 

`echo` is a very useful command-- it is effectively `print` and will report the value of any variable. For example, we just defined the variable file in the above for loop. 

If we now type:

```bash
echo file #prints the name file
echo $file #prints the value of file
```

It will print the value of the variable `$file`. 

Variables are powerful because we can easily manipulate them to do things like find and replace values. Let's make a variable called `santa` and assign it the value `hohoho`. 
```bash
santa=hohoho
echo ${santa}
```
For example, if we wanted to change the letter `o` to an `a` we do this easily with [string manipulations](https://gist.github.com/magnetikonline/90d6fe30fc247ef110a1). 

```bash
echo ${santa//o/a}
```

> Now, what if you wanted to change the name of all the `.out` to `.data`.  Write a for loop to do that. 

### Repeat it with scripts
Ultimately, the thing that makes shell so powerful is your ability to save things you want to do more than once. Navigate into the `scripts` folder. 

Here are some ready made scripts that we are going to take a look at. First type `ls -l`. You should see that all of these scripts are executable. 

Let's take a look at `hello.sh`. 

As you can see it is pretty straight forward. This is a file that contains one command  `echo Hello, world.`. To the screen. 

To execute a bash script, you can use the command `bash`. 

```bash
bash hello.sh
```
This is nice-- but not really the most useful thing in the world. Let's edit this script to allow the user to pass an input to it. There are a series of special variables in shell `$1`, `$2`, etc. These variables correspond to order of the input following the name of the script. The variable `$@` refers to all the variables passed to the script. 

Let's change `hello.sh` so that it will say hello to whatever the input is. Use `nano` and change the input to:

```bash
echo Hello $1! 
```
We can now execute this by typing: 

```bash
bash hello.sh Elmo
```
What is the output of the above? 

> Edit `hello.sh` again to  make it print a hello statement to two people. Note what happens when two inputs aren't provided. 

Now let's look at the script Harriet used to make all the fake data files we have been looking at. Use less to look at `script.sh`. 

```bash
#!/bin/bash
for s in $(seq $1)
do 
        echo $s $RANDOM $RANDOM 
done
```
This script introduces a new useful command `seq`. `seq` will automatically generate a number line from any start position to any end position. The other new thing is the variable `$RANDOM`. What does this do? 

As you can see at the top of the file there is a funny string `#!/bin/bash`. This is called the shebang or bang. It is a common feature of shell scripts-- it tells the computer the path to the interpreter that should be used. This ensures that however you execute your script it will be interpreted with the correct interpreter (i.e. `bash` and `csh` ). 

Now, try running this script-- what do you think you would pass the script? 



### Finding things
#### Finding words in text files
Global regular expression print `grep` is a command that lets you search for words or regular expressions within files. Navigate to `unix-folders-master/writing/fables-poems` and let's give it a try.  We are going to search for the word Cat in the file `LaFontaine.txt`.


```bash
grep Cat LaFontaine.txt
```
As you can see this is printing all the lines that contain the word `Cat` within the file. Now try searching for `cat`. What is the difference? `grep` is a very powerful tool-- use man to take a look at its abilities. Can you figure out how to make it ignore case? 

You can also use `grep` regular expressions. To be safe it is good to append the flag `-E` to the command to force it to read the string passed as a regular expression. 

```bash
grep -E "nose|ring" lear.txt 
```

>Write a command to find all occurrences of two capital letters followed by a space. 

#### Find and replace? 
Often times you will have a file that you want to fix so that it doesn't contain certain character or the like. One option for finding and replacing things within a text file is the command `sed`. 

`sed` is a powerful tool and has had [books](http://shop.oreilly.com/product/9781565922259.do) written about it. Today, we are going to just cover the simplest functionality. Genearlly it takes the form `'s/old_word/new_word/'`. This will replace an old_word with new_word. Let's check this out. Navigate to: `unix-folders-master/writing/fortunes`. 

Let's try replacing `a` with `A` in this file. 

```bash 
sed -e 's/a/A/' fortune5
```

What happened? 

As you can see -- only the first occurrence of `a` was replaced. To replace all the instances of an occurring character in the text file you can add `g` after the search pattern. 

```bash 
sed -e 's/a/A/g' fortune5
```
> Write a `sed` command to find and replace the letters `a` or `s` with `!`.

####Finding files
Sometimes, even with the best organization skills you forget where you put things. While `grep` searches a particular file-- `find` is used to search your file system. Go back to `unix-folders-master/`. 

Try typing `find .`. This is going to to list all the files that exist at this level and below. But, of course, find is much more powerful than that. To find all the directories:

```bash
find . -type d
```
Find can also be used to find files that match a certain name:
```bash
find . -name *.txt
```

---
title: "Working with Files and Directories"
teaching: 30
exercises: 15
questions:
- "How can I view and search file contents?"
- "How can I create, copy and delete files and directories?"
- "How can I control who has permission to modify a file?"
- "How can I repeat recently used commands?"
objectives:
- View, search within, copy, move, and rename files. Create new directories.
- Use wildcards (`*`) to perform operations on multiple files.
- Make a file read only
- Use the `history` command to view and repeat recently used commands.
keypoints:
- "You can view file contents using `less`, `cat`, `head` or `tail`."
- "The commands `cp`, `mv`, and `mkdir` are useful for manipulating existing files and creating new directories."
- "You can view file permissions using `ls -l` and change permissions using `chmod`."
- "The `history` command and the up arrow on your keyboard can be used to repeat recently used commands."
---

## Working with Files

### Our data set: FASTQ files

Now that we know how to navigate around our directory structure, lets
start working with our sequencing files. We did a sequencing experiment and 
have two results files, which are stored in our `data` directory. 

### Wildcards

Navigate to your `data` directory.

~~~
$ cd ~/pan_workshop/data
~~~
{: .bash}

We are interested in looking at the FASTA files in this directory. We can list
all files with the .fastq extension using the command:

~~~
$ ls */*.gbk
~~~
{: .bash}

~~~
agalactiae_18RS21/Streptococcus_agalactiae_18RS21.gbk  agalactiae_H36B/Streptococcus_agalactiae_H36B.gbk
~~~
{: .output}

The `*` character is a special type of character called a wildcard, which can be used to represent any number of any type of character. 
Thus, `*.gbk` matches every file that ends with `.gbk`. 

## Command History

If you want to repeat a command that you've run recently, you can access previous
commands using the up arrow on your keyboard to go back to the most recent
command. Likewise, the down arrow takes you forward in the command history.

A few more useful shortcuts: 

- <kbd>Ctrl</kbd>+<kbd>C</kbd> will cancel the command you are writing, and give you a 
fresh prompt.
- <kbd>Ctrl</kbd>+<kbd>R</kbd> will do a reverse-search through your command history.  This
is very useful.
- <kbd>Ctrl</kbd>+<kbd>L</kbd> or the `clear` command will clear your screen.

You can also review your recent commands with the `history` command, by entering:

~~~
$ history
~~~
{: .bash}

to see a numbered list of recent commands. You can reuse one of these commands
directly by referring to the number of that command.

For example, if your history looked like this:

~~~
479  ls *
480  ls /usr/bin/*.sh
481  ls *.gbk
~~~
{: .output}

then you could repeat command #481 by entering:

~~~
$ !481
~~~
{: .bash}

Type `!` (exclamation point) and then the number of the command from your history.
You will be glad you learned this when you need to re-run very complicated commands.

## Examining Files

We now know how to switch directories, run programs, and look at the
contents of directories, but how do we look at the contents of files?

One way to examine a file is to print out all of the
contents using the program `cat`.
~~~
$ cat filename
~~~

`cat` is a terrific program, but when the file is really big (as the files we have), it can
be annoying to use. The program, `less`, is useful for this
case. `less` opens the file as read only, and lets you navigate through it. The navigation commands
are identical to the `man` program.

Enter the following command:

~~~
$  cd ~/pan_workshop/data/agalactiae_18RS21
$ ls
~~~
{: .bash}

~~~
Streptococcus_agalactiae_18RS21.fna  Streptococcus_agalactiae_18RS21.gbk
~~~
{: .output}

~~~
$ less Streptococcus_agalactiae_18RS21.gbk
~~~
{: .bash}


Some navigation commands in `less`

| key     | action |
| ------- | ---------- |
| <kbd>Space</kbd> | to go forward |
|  <kbd>b</kbd>    | to go backward |
|  <kbd>g</kbd>    | to go to the beginning |
|  <kbd>G</kbd>    | to go to the end |
|  <kbd>q</kbd>    | to quit |

`less` also gives you a way of searching through files. Use the
"/" key to begin a search. Enter the word you would like
to search for and press `enter`. The screen will jump to the next location where
that word is found. 

**Shortcut:** If you hit "/" then "enter", `less` will  repeat
the previous search. `less` searches from the current location and
works its way forward. Note, if you are at the end of the file and search
for the sequence "CAA", `less` will not find it. You either need to go to the
beginning of the file (by typing `g`) and search again using `/` or you
can use `?` to search backwards in the same way you used `/` previously.

For instance, let's search forward for the sequence `TTTTT` in our file. 
You can see that we go right to that sequence, what it looks like,
and where it is in the file. If you continue to type `/` and hit return, you will move 
forward to the next instance of this sequence motif. If you instead type `?` and hit 
return, you will search backwards and move up the file to previous examples of this motif.

Remember, the `man` program actually uses `less` internally and
therefore uses the same commands, so you can search documentation
using "/" as well!

There's another way that we can look at files, and in this case, just
look at part of them. This can be particularly useful if we just want
to see the beginning or end of the file, or see how it's formatted.

The commands are `head` and `tail` and they let you look at
the beginning and end of a file, respectively.

~~~
$  head Streptococcus_agalactiae_18RS21.gbk
~~~
{: .bash}

~~~
LOCUS       AAJO01000169.1          2501 bp    DNA     linear   UNK
DEFINITION  Streptococcus agalactiae 18RS21
ACCESSION   AAJO01000169.1
KEYWORDS    .
SOURCE      Streptococcus agalactiae 18RS21.
  ORGANISM  Streptococcus agalactiae 18RS21
            Bacteria; Terrabacteria group; Firmicutes; Bacilli;
            Lactobacillales; Streptococcaceae; Streptococcus; Streptococcus
            agalactiae.
FEATURES             Location/Qualifiers
~~~
{: .output}

~~~
$ tall Streptococcus_agalactiae_18RS21.gbk
~~~
{: .bash}

~~~
      241 gcagtatcct gagtacggcg agacacgcga aatctcgtcg gaatctggga ggaccatctc
      301 ccaaccctaa atactctcta gtgaccgata gtgaaccagt accgtgaggg aaaggtgaaa
      361 agcaccccgg gaggggagtg aaatagaacc tgaaaccgtg tgcctacaac aagttcgagc
      421 ccgttaatgg gtgagagcgt gccttttgta gaatgaaccg gcgagttacg atatgatgcg
      481 aggttaagtt gaagagacgg agccgtaggg aaaccgagtc ttaatagggc gtcatagtat
      541 catgttgtag acccgaaacc atgtgaccta cccatgagca ggttgaaggt gaggtaaaac
      601 tcactggagg accgaaccag ggcacgttga aaagtgcttg gatgacttgt gggtagcgga
      661 gaaattcaaa cgaacttgga gatagctggt tctctccgaa atagctttag ggctagcgtc
      721 gatgttaagt ctcttggagt aga
//
~~~
{: .output}

The `-n` option to either of these commands can be used to print the
first or last `n` lines of a file. 

~~~
$ head -n 1 Streptococcus_agalactiae_18RS21.gbk
~~~
{: .bash}

~~~
>LOCUS       AAJO01000169.1          2501 bp    DNA     linear   UNK
~~~
{: .output}

~~~
 tail -n 1 Streptococcus_agalactiae_18RS21.gbk
~~~
{: .bash}

~~~
//
~~~
{: .output}

## Details on the GBK format

Although it looks complicated (and it is), it's easy to understand the
[fastq](https://en.wikipedia.org/wiki/FASTA_format) format with a little decoding. Some rules about the format
include...

|Line|Description|
|----|-----------|
|1|May start with a ";" or ">", follows by a name and/or a unique identifier for the sequence, and may also contain additional information|
|2|The actual DNA sequence|
|3|If there are more sequences, it always begins with a '>',  and info like info in line 1|

We can view the first complete read in one of the files our dataset by using `head` to look at
the first four lines.

~~~
$ cd ~/pan_workshop/data/agalactiae_18RS21
$ head -n 10 Streptococcus_agalactiae_18RS21.gbk
~~~
{: .bash}

~~~
LOCUS       AAJO01000169.1          2501 bp    DNA     linear   UNK
DEFINITION  Streptococcus agalactiae 18RS21
ACCESSION   AAJO01000169.1
KEYWORDS    .
SOURCE      Streptococcus agalactiae 18RS21.
  ORGANISM  Streptococcus agalactiae 18RS21
            Bacteria; Terrabacteria group; Firmicutes; Bacilli;
            Lactobacillales; Streptococcaceae; Streptococcus; Streptococcus
            agalactiae.
FEATURES             Location/Qualifiers
~~~
{: .output}

## Creating, moving, copying, and removing

Now we can move around in the file structure, look at files, and search files. But what if we want to copy files or move
them around or get rid of them? Most of the time, you can do these sorts of file manipulations without the command line,
but there will be some cases (like when you're working with a remote computer like we are for this lesson) where it will be
impossible. You'll also find that you may be working with hundreds of files and want to do similar manipulations to all 
of those files. In cases like this, it's much faster to do these operations at the command line.


### Copying Files

When working with computational data, it's important to keep a safe copy of that data that can't be accidentally overwritten or deleted. 
For this lesson, our raw data is our GBK files.  We don't want to accidentally change the original files, so we'll make a copy of them
and change the file permissions so that we can read from, but not write tcdo, the files.

First, let's make a copy of one of our GBK files using the `cp` command. 

Navigate to the `~/pan_workshop/data/agalactiae_18RS21` directory and enter:

~~~
$ for i in $(ls -d */); do cd $i; for j in $(ls *.fasta); do cp $j copy-$j  ; done; cd ..; done
$ for i in $(ls -d */); do cd $i; ls -F; cd ..; done
~~~
{: .bash}

~~~
copy-Streptococcus_agalactiae_18RS21.fasta  Streptococcus_agalactiae_18RS21.gbk Streptococcus_agalactiae_18RS21.fasta

copy-Streptococcus_agalactiae_515.fasta  Streptococcus_agalactiae_515.gbk Streptococcus_agalactiae_515.fasta

copy-Streptococcus_agalactiae_A909.fasta  Streptococcus_agalactiae_A909.gbk Streptococcus_agalactiae_A909.fasta

copy-Streptococcus_agalactiae_CJB111.fasta  Streptococcus_agalactiae_CJB111.gbk Streptococcus_agalactiae_CJB111.fasta

copy-Streptococcus_agalactiae_COH1.fasta  Streptococcus_agalactiae_COH1.gbk Streptococcus_agalactiae_COH1.fasta

copy-Streptococcus_agalactiae_H36B.fasta  Streptococcus_agalactiae_H36B.gbk Streptococcus_agalactiae_H36B.fasta
~~~
{: .output}

We now, for example, have two copies of the `Streptococcus_agalactiae_18RS21.fasta` file, one of them named `copy-Streptococcus_agalactiae_18RS21.fasta`. We'll move this file to a new directory called `backup` where we'll store our backup data files.

### Creating Directories

The `mkdir` command is used to make a directory. Enter `mkdir`
followed by a space, then the directory name you want to create.

~~~
$ mkdir backup
~~~
{: .bash}

### Moving / Renaming 

We can now move our backup file to this directory. We can
move files around using the command `mv`. 

~~~
$ for i in $(ls -d */); do cd $i; mv copy-* ~/gm_workshop/backup; cd ..; done
$ ls backup
~~~
{: .bash}
 
~~~
copy-Streptococcus_agalactiae_18RS21.fasta
copy-Streptococcus_agalactiae_515.fasta
copy-Streptococcus_agalactiae_A909.fasta
copy-Streptococcus_agalactiae_CJB111.fasta
copy-Streptococcus_agalactiae_COH1.fasta
copy-Streptococcus_agalactiae_H36B.fasta
~~~
{: .output}

The `mv` command is also how you rename files. Let's rename this file to make it clear that this is a backup.

~~~
$ cd backup
$ mv copy-Streptococcus_agalactiae_18RS21.fasta backup-Streptococcus_agalactiae_18RS21.fasta
$ ls
~~~
{: .bash}

~~~
backup-Streptococcus_agalactiae_18RS21.fasta
copy-Streptococcus_agalactiae_515.fasta
copy-Streptococcus_agalactiae_A909.fasta
copy-Streptococcus_agalactiae_CJB111.fasta
copy-Streptococcus_agalactiae_COH1.fasta
copy-Streptococcus_agalactiae_H36B.fasta
~~~
{: .output}

** Aquí voy (falta ver como cambiarle el nombre a todos los archivos en un solo ciclo)**

### Removing

When we want to remove a file or a directory we use the `rm` command. By default, `rm`will not delete directories.
You can tell `rm` to delete a directory using the `-r` (recursive) option. 

Let's delete the backup directory we just made. 
~~~
$ cd ..
$ rm -r backup
~~~
{: .bash}

This will delete not only the directory, but all files within the directory. If you have write-protected files in the directory, 
you will be asked whether you want to override your permission settings. 


If we want to modifiy a file without all the permissions you'll be asked if you want to override your file permissions.
for example:

~~~
rm: remove write-protected regular file ‘example.fastq’? 
~~~
{: .output}

If you enter `n` (for no), the file will not be deleted. If you enter `y`, you will delete the file. This gives us an extra 
measure of security, as there is one more step between us and deleting our data files.

**Important**: The `rm` command permanently removes the file. Be careful with this command. It doesn't
just nicely put the files in the Trash. They're really gone.

> ## Exercise 1: Make backup folder with write-protected permissions
>
> Starting in the `/home/dcuser/dc_workshop/data/untrimmed_fastq` directory, do the following:
> 1. Make sure that you have deleted your backup directory and all files it contains.  
> 2. Create a copy of each of your FASTQ files. (Note: You'll need to do this individually for each of the two FASTQ files. We haven't 
> learned yet how to do this
> with a wildcard.)  
> 3. Use a wildcard to move all of your backup files to a new backup directory.   
> 4. Change the permissions on all of your backup files to be write-protected.  
>
> > ## Solution
> >
> > 1. `rm -r backup`  
> > 2. `cp JC1A_R1.fastq JC1A_R1-backup.fastq`, `cp JC1A_R2.fastq JC1A_R2-backup.fastq`, `cp JP4D_R1.fastq JP4D_R1-backup.fastq`  
> > and `cp JP4D_R2.fastq JP4D_R2-backup.fastq` 
> > 3. `mkdir backup` and `mv *-backup.fastq backup`
> > 4. `chmod -w backup/*-backup.fastq`   
> > It's always a good idea to check your work with `ls -l backup`. You should see something like: 
> > 
> > ~~~
> > -r--r--r-- 1 dcuser dcuser  24203913 Jun 17 23:08 JC1A_R1-backup.fastq
> > -r--r--r-- 1 dcuser dcuser  24917444 Jun 17 23:10 JC1A_R2-backup.fastq
> > -r--r--r-- 1 dcuser dcuser 186962503 Jun 17 23:10 JP4D_R1-backup.fastq
> > -r--r--r-- 1 dcuser dcuser 212161034 Jun 17 23:10 JP4D_R2-backup.fastq
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

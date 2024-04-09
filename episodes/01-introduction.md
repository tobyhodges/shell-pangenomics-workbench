---
title: "Introducing the Shell"
teaching: 20
exercises: 5
questions:
- "What is a command shell and why would I use one?"
- "How can I move around in a computer?"
- "How can I see what files and directories I have?"
- "How can I specify the location of a file or directory on my computer?"
objectives:
- "Describe key reasons for learning shell."
- "Learn how to access a remote machine."
- "Navigate your file system using the command line."
- "Access and read help files for `bash` programs and use help files to identify useful command options."
- "Demonstrate the use of tab completion, and explain its advantages."
keypoints:
- "The shell gives you the ability to work more efficiently by using keyboard commands rather than a GUI."
- "Useful commands for navigating your file system include: `ls`, `pwd`, and `cd`."
- "Most commands take options (flags) which begin with a `-`."
- "Tab completion can reduce errors from mistyping and make work more efficient in the shell."
---

## What is a shell and why should I care?

A *shell* is a computer program that presents a command line interface
which allows you to control your computer using commands entered
with a keyboard instead of controlling graphical user interfaces
(GUIs) with a mouse/keyboard combination.

There are many reasons to learn about the shell.

* Many bioinformatics tools can only be used through a command line interface, or 
have extra capabilities in the command line version that are not available in the GUI.
This is true, for example, of BLAST, which offers many advanced functions only accessible
to users who know how to use a shell.  
* The shell makes your work less boring. In bioinformatics you often need to do
the same set of tasks with a large number of files. Learning the shell will allow you to
automate those repetitive tasks and leave you free to do more exciting things.  
* The shell makes your work less error-prone. When humans do the same thing a hundred different times
(or even ten times), they're likely to make a mistake. Your computer can do the same thing a thousand times
with no mistakes.  
* The shell makes your work more reproducible. When you carry out your work in the command-line 
(rather than a GUI), your computer keeps a record of every step that you've carried out, which you can use 
to re-do your work when you need to. It also gives you a way to communicate unambiguously what you've done, 
so that others can check your work or apply your process to new data.  
* Many bioinformatic tasks require large amounts of computing power and can't realistically be run on your
own machine. These tasks are best performed using remote computers or cloud computing, which can only be accessed
through a shell.

In this lesson you will learn how to use the command line interface to move around in your file system. 

## How to access the shell

On a Mac or Linux computer, you can access a shell through a program called Terminal, which is already available
on your computer. If you're using Windows, you'll need to download a separate program to access the shell (see installation instructions [here](https://carpentries-incubator.github.io/metagenomics-workshop/setup.html)). If you have all the bioinformatic programs installed in your local machine you are all set.

In this workshop, we suggest using a remote server to invest most of our time learning the basics of shell by manipulating some experimental data. The remote server already includes the required bioinformatics packages as well as the large datasets that usually take a lot of time to load into everyone's local computers. If you are in a workshop from UNAM-CCM you will access the Bash shell and the Python notebook through a JupyterHub server. If you are in another workshop you will access them through an AWS remote machine.

### Connection to the UNAM-CCM JupyterHub
Open the JupyterHub server login site in a new tab with [this link](https://lab.matmor.unam.mx:8443/hub/login).  
Open this [Google sheet](https://docs.google.com/spreadsheets/d/1Lg633gpV8KrUqTn34PDM8V0glAkTZpHcm4CViRRz1N0/edit#gid=1473209790) in a new tab and write your name in a user.  
Use this user information to log in to the JupyterHub site that you opened in the previous step.  
To open a Bash Terminal click on the button "New" (upper right within the "Files" section) and choose the option "Terminal" from the drop-down menu. A new tab with a terminal will open.  

### Connection to the AWS remote machine
The instructor of your workshop will give you an `ip_address` and password to login.

To log in you need to open the Terminal program and use the `ssh` command (ssh stands for Secure Shell), your username and the address of the machine you are logging into.
~~~
$ ssh dcuser@ec2-18-702-132-236.compute-1.amazonaws.com
~~~
{: .bash}

Then you are prompted to type the password. Take into account that while you are typing a password no characters will appear on the screen, trust that they are being typed and press enter. 

After logging in, you will see a screen showing something like this: 

~~~
Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-48-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Sat Feb  2 00:08:17 UTC 2019

  System load: 0.0                Memory usage: 5%   Processes:       82
  Usage of /:  29.9% of 98.30GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

597 packages can be updated.
444 updates are security updates.

New release '16.04.5 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


Last login: Fri Feb  1 22:34:53 2019 from c-73-116-43-163.hsd1.ca.comcast.net
~~~
{: .output}

This provides a lot of information about the remote server that you're logging in to. We're not going to use most of this information for
our workshop, so you can clear your screen using the `clear` command. 

~~~
$ clear
~~~
{: .bash}

This will scroll your screen down to give you a fresh screen and will make it easier to read. 
You haven't lost any of the information on your screen. If you scroll up, you can see everything that has been output to your screen
up until this point.

## Navigating your file system

> ## Prepare your genome database
> Make sure you have the `pan_workshop/` directory. If you do not have it, you can download it with the following instructions.
>
> ~~~
> $ cd ~ #Make sure you are in the home directory
> $ wget https://zenodo.org/record/7974915/files/pan_workshop.zip?download=1 #Download the `zip` file.
> $ unzip 'pan_workshop.zip?download=1' 
> $ rm 'pan_workshop.zip?download=1'
> ~~~
> {: .language-bash}
{: .checklist}


The part of the operating system responsible for managing files and directories
is called the **file system**.
It organizes our data into files,
which hold information,
and directories (also called "folders"),
which hold files or other directories.

Several commands are frequently used to create, inspect, rename, and delete files and directories.

> ## Preparation Magic
>
> If you type the command:
> `PS1='\W\$ '`
> into your shell, followed by pressing the <kbd>Enter</kbd> key,
> your window should look like this:    
> `~\$ `   
> That only shows the ultimate directory where you ar standing. In this case
> it is the home directory. The symbol `~` is an abbreviation of the home directory. 
> This isn't necessary to follow along (in fact, your prompt may have
> other helpful information you want to know about).  This is up to you!  
{: .callout}

The dollar sign is a **prompt**, which shows us that the shell is waiting for input;
your shell may use a different character as a prompt and may add information before
the prompt. When typing commands, either from these lessons or from other sources,
do not type the prompt, only the commands that follow it. In this lesson we will use the 
dollar sign to indicate the prompt. 

~~~
$
~~~
{: .bash}


Let's find out where we are by running a command called `pwd`
(which stands for "print working directory").
At any moment, our **current working directory**
is our current default directory,
i.e.,
the directory that the computer assumes we want to run commands in
unless we explicitly specify something else.
Here,
the computer's response is `/home/dcuser`,
which is the top level directory within our cloud system:

~~~
$ pwd
~~~
{: .bash}

~~~
/home/dcuser
~~~
{: .output}

Let's look at how our file system is organized. We can see what files and subdirectories are in this directory by running `ls`,
which stands for "listing":

~~~
$ ls
~~~
{: .bash}

~~~
pan_workshop   
~~~
{: .output}

`ls` prints the names of the files and directories in the current directory in
alphabetical order, arranged neatly into columns. 
We'll be working within the `pan_workshop` subdirectory, and creating new subdirectories, 
throughout this workshop.  

The command to change locations in our file system is `cd` followed by a
directory name to change our working directory.
`cd` stands for "change directory".

Let's say we want to navigate to the `pan_workshop` directory we saw above.  We can
use the following command to get there:

~~~
$ cd pan_workshop
~~~
{: .bash}

Let's look at what is in this directory:

~~~
$ ls
~~~
{: .bash}

~~~
data  
~~~
{: .output}

We can make the `ls` output more comprehensible by using the **flag** `-F`,
which tells `ls` to add a trailing `/` to the names of directories:

~~~
$ ls -F
~~~
{: .bash}

~~~
data/  
~~~
{: .output}

Anything with a "/" after it is a directory. Things with a "*" after them are programs. If
there are no decorations, it's a file.

To understand a little better how to move between folders, let's see the following image:

<a href="{{ page.root }}/fig/02-01-01.png">
  <img src="{{ page.root }}/fig/02-01-01.png" width="870" height="631" alt="Folder organization example diagram"/>
</a>

Here we can see a diagram of how the folders are arranged one inside another. In this way, if we think about moving,
from `pan_workshop` to the `data` folder, the path must go as they are ordered: `cd pan_workshop/data`

`ls` has lots of other options. To find out what they are, we can type:

~~~
$ man ls
~~~
{: .bash}

Some manual files are very long. You can scroll through the file using
your keyboard's down arrow or use the <kbd>Space</kbd> key to go forward one page
and the <kbd>b</kbd> key to go backwards one page. When you are done reading, hit <kbd>q</kbd>
to quit.

> ## Exercise 1: Extra information with `ls -l`
> Use the `-l` option for the `ls` command to display more information for each item 
> in the directory. What is one piece of additional information this long format
> gives you that you don't see with the bare `ls` command?
>
> > ## Solution
> > ~~~
> > $ ls -l
> > ~~~
> > {: .bash}
> > 
> > ~~~
> > total 4
> > drwxr-xr-x 4 dcuser dcuser 4096 feb 17 12:42 data
> > ~~~
> > {: .output}
> > 
> > The additional information given includes the name of the owner of the file,
> > when the file was last modified, and whether the current user has permission
> > to read and write to the file.
> > 
> {: .solution}
{: .challenge}

No one can possibly learn all of these arguments, that's why the manual page
is for. You can (and should) refer to the manual page or other help files
as needed.

Let's go into the `data/agalactiae_H36B` directory and see what is in there.

~~~
$ cd data/agalactiae_H36B
$ ls -F
~~~
{: .bash}

~~~
Streptococcus_agalactiae_H36B.fna  Streptococcus_agalactiae_H36B.gbk
~~~
{: .output}

### Shortcut: Tab Completion

Usually the key Tab is located on the left side of the keyboard just above the "Shift" key or "Mayus" key. 

Typing out file or directory names can waste a
lot of time and it's easy to make typing mistakes. Instead we can use tab complete 
as a shortcut. When you start typing out the name of a directory or file, then
hit the <kbd>Tab</kbd> key, the shell will try to fill in the rest of the
directory or file name.

Return to your home directory:

~~~
$ cd
~~~
{: .bash}

then enter:

~~~
$ cd pan<tab>
~~~
{: .bash}

The shell will fill in the rest of the directory name for
`pan_workshop`.

Now change directories to `data` in `pan_workshop`

~~~
$ cd pan_workshop
$ cd data
~~~
{: .bash}

Using tab complete can be very helpful. However, it will only autocomplete
a file or directory name if you've typed enough characters to provide
a unique identifier for the file or directory you are trying to access.

~~~
$ ls ag<tab>
~~~
{: .bash}

The shell auto-completes your command to `agalactiae_`, because there is another name in 
the directory begin with this prefix. When you hit
<kbd>Tab</kbd> again, the shell will list the possible choices.

~~~
$ ls  ag<tab><tab>
~~~
{: .bash}

~~~
agalactiae_18RS21/ agalactiae_H36B/
~~~
{: .output}

Tab completion can also fill in the names of programs, which can be useful if you
remember the beginning of a program name.

~~~
$ pw<tab><tab>
~~~
{: .bash}

~~~
pwd   pwdx
~~~
{: .output}

Displays the name of every program that starts with `pw`. 

## Summary

We now know how to move around our file system using the command line.
This gives us an advantage over interacting with the file system through
a Graphical User Interface (GUI) as it allows us to work on a remote server, carry out the same set of operations 
on a large number of files quickly, and opens up many opportunities for using 
bioinformatics software that is only available in command line versions. 

In the next few episodes, we'll be expanding on these skills and seeing how 
using the command line shell enables us to make our workflow more efficient and reproducible.

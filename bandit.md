# Bandit excercises (9/17/2024)

## James Commons

### Level 0
Password: bandit0

### Level 0 -> level 1

Password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

Steps:

1. I just ran `cat readme`

This level just familarizes us with reading a file from the commandline using
cat.

### Level 1 -> level 2

Password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

Steps:

1. Run `cat < -` to redirect stdin to -

Some files have weird names that mean different things to the shell. This
excercise teaches us that sometimes we need to find creative ways to get around
this.

### Level 2 -> level 3

Password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

Steps:

1. Run `cat "<filename>"` becauase there are spaces

You can also put an entire file name in quotes if there are spaces in it. This
comes up a lot, especially if the system has GUI where the user might be
tempted to put spaces in filenames.

### Level 3 -> level 4

Password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

Steps:

1. Changed to inhere directory
1. Ran ls -la to find hidden directory
1. cat its contents

This excercise gets us familiar with changing directories and hidden files.

### Level 4 -> level 5

Password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

Steps:
1. cd into inhere
1. Ran `strings ./*` and found a password

I'm sure there are many ways to do this puzzle, but it demonstrates the power
of using the commandline instead of a GUI for certain operations.

### Level 5 -> level 6

Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

Steps:
1. cd into inhere
1. Ran `ls -lRAs`, which listed file sizes, and used that info
to find the password file

This excercise gets us familar with some more ls options and also looking for
files of a certain size.

### Level 6 -> level 7

Password: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

Steps:
1. I ran `find / -size 33c -exec ls -la {} +`
1. The first file listed contained the password

Find and locate are powerful tools for efficiently finding file that you are
looking for. They are used a lot in Unix, and their syntax is clearly a little
strange, so getting familiar with them is super useful.

### Level 7 -> level 8

Password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

Steps:
1. I used less to find the world millionth in data.txt

Again, there are so many ways to doing this, but this excercise teaches us how
to use things like regex to find strings in a file.

### Level 8 -> level 9

Password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

Steps:
 1. Ran `sort data.txt | uniq -u`

This introduces us to another command in Unix. I didn't know this one existed,
so I'm glad that there was a hint on the website.

### Level 9 -> level 10

Password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

Steps:
1. Literally just ran strings data.txt

My guess is that this is the first excercise where we were supposed to used
strings. This command is super useful for looking for information in binary
files like executables. These files often contain raw data that can be useful
to an attacker, even if the file itself is not meant to be readable.

### Level 10 -> level 11

Password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

Steps:

1. Ran `base64 -d data.txt`

This just familiarizes us with base64 and the associated tools in Unix.

### Level 11 -> level 12

Password: 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

Steps:

1. This is just a Caesar cipher, so I looked up a decoder online

I'm sure there are many ways to solve this one, inluding writing your own
script, but I happend to know what this type of encoding is called. I think
this excercise teaches us that there are many tools that are useful, and not
all of them have to be from the command line. If what you do works, then great.


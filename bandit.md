# Bandit excercises (9/17/2024)

## James Commons

### Level 0
Password: bandit0

### Level 0 -> level 1

Password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

Steps:

I just ran `cat readme`

### Level 1 -> level 2

Password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

Steps:

Run `cat < -` to redirect stdin to -

### Level 2 -> level 3

Password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

Steps:

Run `cat "<filename>"` becauase there are spaces

### Level 3 -> level 4

Password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

Steps:

1. Changed to inhere directory
1. Ran ls -la to find hidden directory
1. cat its contents

### Level 4 -> level 5

Password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

Steps:
1. cd into inhere
1. Ran `strings ./*` and found a password

### Level 5 -> level 6

Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

Steps:
1. cd into inhere
1. Ran `ls -lRAS`, which listed file sizes, and used that info
to find the password file

### Level 6 -> level 7

Password: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

Steps:
1. I ran `find / -size 33c -exec ls -la {} +`
1. The first file listed contained the password:w

### Level 7 -> level 8

Password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

Steps:
1. I used less to find the world millionth in data.txt

### Level 8 -> level 9

Password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

Steps:
 1. Ran `sort data.txt | uniq -u`

### Level 9 -> level 10

Password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

Steps:
1. Literally just ran strings data.txt

### Level 10 -> level 11

Password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

Steps:

1. Ran `base64 -d data.txt`

### Level 11 -> level 12

Password: 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

Steps:

1. This is just a Caesar cipher, so I looked up a decoder online


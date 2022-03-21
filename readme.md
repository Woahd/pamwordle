 pam-wordle

Wordle - but as a Linux-PAM module!

## Usage

Note: Tested on `Ubuntu 20.04.4`. This documentation may not be accurate for other systems.

### Install

Requirements: `gcc` and `libpam0g-dev`

`apt install gcc libpam0g-dev -y`

Compile and link:

```
gcc -fPIC -c wordle.c
sudo ld -x --shared -o /lib/x86_64-linux-gnu/security/pam_wordle.so wordle.o
```

### Configuration

In order to use pam-wordle you must edit the Linux-PAM configuration in `/etc/pam.d`.

For example, if you wish to ensure all sudo users know their vocabulary, replace `/etc/pam.d/sudo` with the following:

```
session    required   pam_env.so readenv=1 user_readenv=0
session    required   pam_env.so readenv=1 envfile=/etc/default/locale user_readenv=0
auth    required pam_unix.so
auth    required    pam_wordle.so
@include common-account
@include common-session-noninteractive
```

### Dictionary

pam-wordle uses 5 character long words from `/usr/share/dict/words` for its dictionary.

If the dictionary is not present pam-wordle will complain via error messages and only use the word `linux`.

An American English wordlist can be installed via the `wamerican` package:

`apt install wamerican -y`



###Example
```
[user@exmapleserver]# ssh 192.168.1.100
--- Welcome to PAM-Wordle! ---

A five character [a-z] word has been selected.
You have 6 attempts to guess the word.

After each guess you will recieve a hint which indicates:
? - what letters are wrong.
* - what letters are in the wrong spot.
[a-z] - what letters are correct.

--- Attempt 1 of 6 ---
Word: crane
Hint->????*
--- Attempt 2 of 6 ---
Word: boats
Hint->???**
--- Attempt 3 of 6 ---
Word: steam
Hint->s**??
--- Attempt 4 of 6 ---
Word: swept
Hint->s?***
--- Attempt 5 of 6 ---
Word: setup
Correct!
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 5.4.0-1068-aws x86_64)
```

![](media/b80e0eacca6dad9d42b5dc3545946591.png)

Password Cracking
=================================

Overview
========

The goal of this lab is to familiarize students with password files and some simple password cracking schemes.

Lab Environment
===============

Once you have logged into your range account and accessed your Labtainer-VM,
open a terminal window.

Navigate to the “labtainer-student” directory and start the lab using the
command:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
>   labtainer pass-crack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   Links to this lab manual will be displayed if you wish to view the prompt
    from within your VM

**Note**: There is an appendix of some helpful commands at the end of these
instructions.

Task 1: Password Files
======================

In this task, you will briefly examine how your Linux system manages and stores user passwords.

-  Use the more command, as shown below, to view the /etc/passwd file.

```
>   more /etc/passwd
```

- You should see a list of all the users that have login access to your machine. At the bottom of this file you should see a line for your account (e.g., the “student” account).

- There was a time when /etc/passwd stored password digests, but you’ll notice no digests exist in this file. Modern Linux/Unix distrobutions use what is called a *shadow passwords* file. This is a special file managed by the operating system to store and protect password digests.

- Your shadow password file exists at /etc/shadow. Use the "more" command, as shown below to *try* to view its contents.

```
>   more /etc/shadow
```
*Note the error message you received when you tried to view the shadow password file.

- Try opening the shadow password file with root privileges. You can gain root privileges on Ubuntu by preceding your command with "sudo". 

```
>   sudo more /etc/shadow
```
-  After entering your password you should be able to see the contents of the shadow password file.

- Go to the bottom of the shadow password file to see the entry for your account (which may be so long it wraps around to the next line).


Each line in the shadow file is separated into different fields by a ‘:’.

Reading left-to-right, the fields are:
```
    1.  Login name

    2.  Digest (referred to horribly as the “encrypted password”)

    3.  Date of last password change

    4.  Minimum password age

    5.  Maximum password age

    6.  Password warning period

    7.  Password inactivity period

    8.  Account expiration date

    9.  Reserved
```
Within the field designated for the digest, it is further broken up into other fields that are separated by a ‘\$’. These fields are:

```
    **\$**ID**\$**salt**\$**digest
```
The “ID” field contains a number that corresponds to the hash function/algorithm used to generate the digest. The following table shows
the interpretation of that number:

| **ID** | **Hash Function** |
|--------|-------------------|
| 1      | MD5               |
| 2      | Blowfish          |
| 3      | NT-hash           |
| 4      | (not used)        |
| 5      | SHA-256           |
| 6      | SHA-512           |


*Using the information provided above, determine the hash function that was used to generate the value currently stored in your entry of the shadow password file and the salt value.

- Execute the following command to list some account information (where the "-l” the letter, not the number one):

```
    chage -l student
```

*Note the date when your password was chosen.

Task 2: Dictionary Attacks
==========================

In this task, you will try a dictionary attack to crack the contents of a password file.

- Use the "cat" command to view the contents of the htpasswd-sha1 file.

You will notice that this is not, in fact, a Linux/Unix-like password file. It is actually an htpasswd format, which can be used by an Apache web server to provide password-based access control to portions of a web site. The line:
```
>   alice:{SHA}A9Z8JjwnpFPvZbKeMDNHJzM8y80=
```
says the user “alice” has had her password hashed by the SHA1 hash function, and that the digest is stored in a base64 encoded digest of
“A9Z8JjwnpFPvZbKeMDNHJzM8y80=”.

- Examine the digest values for each user.

*Note the users that selected the same password.

Task 3: Simple Dictionary Attack
==========================

In this task you will be performing a simple and literal dictionary attack because the list contains only about 109k english words; no variations of words are attempted.

- Use the "crackSHA.py" script with the following command to execute a dictionary attack on the "htpasswd-sha1" passowrd digest file, using the list of english word saved in the file "tinylist.txt":

```
./crackSHA.py htpasswd-sha1 tinylist.txt
```

*Note the username(s) and password(s) of the accounts that were cracked.

Task 4: Common Password Dictionary Attack
==========================

By now, you should have cracked some passwords, but not all. This is because the dictionary you used contains only a small set of English words, with no common passwords or variants. Instead of the rather small list of words used above, you will now use biglist.txt that you downloaded earlier, which contains 2.2 million words and commonly used passwords.

- Execute the crackSHA.py script again with the new dictionary (biglist.txt), as shown below:
```
>   ./crackSHA.py htpasswd-sha1 biglist.txt
```

*Note the username(s) and password(s) of the accounts that were cracked when using biglist.txt.

*Note the number of words that were attempted, the number of passwords that were cracked, and the number of secondsit took (as reported at the end of the output).

**In this Lab, there are at least two things slowing down the password cracking: 1) running in a VM; and 2) executing via an interpreted script (rather than a compiled program). Otherwise the rate should be higher!

Task 5: Considering Execution Time
==================================

In this task you will be comparing the execution time of various hash functions.

- As a speed comparison between different hash functions, execute the following script on a file that hashed the passwords using MD5 (an older hash function):
```
>   ./crackMD5.py htpasswd-md5 biglist.txt
```

*Note the number of words that were attempted, the number of passwords that were cracked, and the number of seconds it took. Consider the benefits of salt values.

- As another point of reference in the speed comparison, execute the following script on a password file that used SHA512:
```
>   ./crack512.py htpasswd-sha512 biglist.txt
```

*Note the number of words that were attempted, the number of passwords that were cracked, and the number of seconds it took.


Task 6: Considering Execution Time
==================================

In the next step below you will execute a script that will make use of
pre-calculated digests. These pre-calculated digests were created by hashing
each password in biglist.txt with SHA1, and then sorting the words into files
whose names were the first two hex digits of the digest. No digests were saved.

To use the presorted passwords, the logic of the password-cracking script is
roughly as follows:

a. Get a digest from the password file and look at the first two hex digits.

b. Open the file that has the same name as those two hex digits.

c. Hash each word in the opened file to see if one of the words will hash to the
same full digest as was seen in step ‘a’.

1.  Do the following to see all the **file names** for the sorted dictionary
    words:

    ls calc

2.  Do the following to see the contents of **one** of the saved files:

    more calc/a9

    This file contains all the words in biglist.txt that hash to a digest that
    starts with “a9”. If a digest in a password file starts with “a9”, then the
    script only needs to hash the words in this file to **potentially** find a
    match.

3.  Execute the following command to make use of the pre-calculated digests:

    ./crackPre.py htpasswd-sha1 calc

>   **Record in item \#18 of the worksheet the number of words that were
>   attempted, the number of passwords that were cracked, and the number of
>   seconds it took.**

>   **Note that item \#19 of the worksheet asks a follow-up question.**

Task 7: Personal Experimentation
================================

In this task you will experiment with passwords of your choice.

As described below, create your own password file (named htpasswd-me) in the htpasswd format, with an entry for “alice”. You will be prompted for the password.

```
> htpasswd **-sc** htpasswd-me alice
```
You can **add** other entries by doing the following (slightly modified) command:

```
> htpasswd -s htpasswd-me bob
```

As a security exercise, you may want to add passwords you commonly use (or have used) to see if they can be cracked using this relatively small list of passwords.

- Display your htpasswd-me file by using the cat command.

- Perform the pre-calculated attack as follows:
```
    ./crackPre.py htpasswd-me calc
```
*Note the results of your experiments. 

Submission
==========

After finishing the lab, go to the terminal on your Linux system that was used to start the lab and type:

```
stoplab pass-crack
```
When you stop the lab,
the system will display a path to the zipped lab results on your Linux system.
Provide that file to your instructor, e.g., via the Sakai site.

Appendix – Some Unix Commands
=============================

| cd    | Change the current directory. cd destination With no “destination” your current directory will be changed to your home directory. If you “destination” is “..”, then your current directory will be changed to the parent of your current directory. |
|-------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|                                                        |
| clear | Erase all the output on the current terminal and place the shell prompt at the top of the terminal.                                                                                                                                                  |                                    |
| ls    | List the contents and/or attributes of a directory or file ls location ls file With no “location” or “file” it will display the contents of the current working directory.                                                                           |                                  |
| more  | Display a page of a text file at a time in the terminal. (Also see less). more file To see another page, press the space bar. To see one more line, press the Enter key. To quit at any time press ‘q’ to quit.                                      |
| pwd   | Display the present working directory pwd                                                                                                                                                                                                            |

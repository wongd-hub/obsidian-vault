#course_cs50 

- To be secure, we need to control who has access to something and it has to be resistant to attack.
    - *Authentication* is the act of logging in
    - *Authorisation* is the act of deciding what privileges a user has

- People aren't very good at choosing passwords and tend to choose simple patterns that meet the minimum password requirements. This means if a hacker is attempting to penetrate your account, they'll likely try the most common passwords first (a dictionary attack).
    - We're also in the habit of re-using passwords since remembering new, well-written passwords is difficult. 
    - That's why we have *password managers* now that retain your passwords and create new, secure passwords for you. But what if the password to your password manager is figured out?

- It is naive to say that something is completely secure against penetration, since an adversary with enough time, motivation, and resources can surely get into most any system.
    - We have to be perfectly secure, since all it takes is one hole in the system for an adversary to get in.
    - That's why a lot of cybersecurity focuses on creating a gauntlet of defenses or at least detecting the adversary.
    - The trade-off to you is time and convenience.

# 4-digit passcodes

- Phones nowadays are requiring passcodes of at least 4 numerical digits. How secure are they?
    - It takes a few seconds, maybe even faster, to crack a passcode with 4 digits in it.

- There are $10^4 = 10,000$ different combinations of 4-digit passcodes that are possible. 
# Brute force attacks

- An attack where every possible combination of the password is tried.
- In the case of a 4-digit passcode - something in the spirit of this:

```python
from string import digits
from itertools import product # Cross-product function

for passcode in product(digits, repeat=4):
    print(passcode)

#$ python crack.py
#> Takes less than a second
```

- In the case of a 4-letter (upperand lower-case) passcode though, we have $52^4 \approx 7,000,000$ possibilities. 

```python
from string import ascii_letters
from itertools import product # Cross-product function

for passcode in product(ascii_letters, repeat=4):
    print(passcode)

#$ python crack.py
#> Still takes a few seconds, and less than a minute
```

- Now if we add in punctuation and numbers to the mix (so letters, punctuation, numbers) and we increase the length of the passcode to 8 characters, we have $94^4 \approx 6 \text{ quadrillion}$.

```python
from string import ascii_letters, digits, punctuation
from itertools import product # Cross-product function

for passcode in product(ascii_letters + digits + punctuation, repeat=8):
    print(passcode)

#$ python crack.py
#> Clearly is better, but will take finite time.
```

- However, phones lock you down once you get a certain number of incorrect tries. This significantly increases the cost to the hacker/adversary.
# Two-factor authentication / One-time passwords

- This adds a second, fundamentally different factor to the mix when authenticating.
    - Passwords are one thing, but the second factor in addition to that will be a text to your phone, or a one-time passcode from an authentication software.
    - The password is something you *know*, whereas the second factor is likely to be something you *have*.

- The one-time passcodes you get will also expire with time, adding a further layer of security to them.

# Hashing / encryption

- We started thinking about this concept when we started [[Cryptography]].

- If you ever start a forgotten password process with a website that then sends you your unencrypted password in an email, that is horrible design.
    - Websites should *not* be storing passwords that are unobfuscated from the user's actual password.
    - The mechanism that websites use to obfuscate passwords is called *hashing*.

- On the server side, there will be a database or a text file that associates usernames with passwords. 
    - However, if those passwords were not obfuscated, then any hacker that gets their hands on this has an easy way in. They can further use these passwords in what is referred to as *password stuffing*, where the assumption is that you've re-used the same password on multiple sites.

- See [[Hashing#In the context of passwords]] for details on how this works.




---
layout: post
title: "Understanding Password Attacks: A Beginner's Guide"
date: 2022-07-03 12:00:00 +0530
categories: [Cybersecurity]
tags: [password attack, bruteforce, dictionary, hybrid, rainbow table, credential stuffing]
comments: true
---

Cybersecurity professionals and attackers both rely on understanding how passwords can be broken. This blog explains common types of **password attacks** used in the real world.

---

## Bruteforce Attack

Bruteforce attacks try every possible combination of characters until the correct password is found.

For example:

```
1234  
1234567890  
00000000  
```

### Create a Password List

If you know the password is made only of numbers, use digits `0-9` to generate all combinations.

```bash
crunch 8 8 0123456789 -o num8.txt
```

This will generate an 8-digit number password list.

Then use tools like **Hydra**, **Medusa**, or **John the Ripper** to attack the login page.

---

## Dictionary Attack

Dictionary attacks use a file (wordlist) with commonly used passwords.

Example from `rockyou.txt`:
```
happynewyear  
fuckyou  
password  
admin123  
```

You can create your own dictionary file:

```bash
nano mywords.txt
```

Then use the list to try login combinations using your preferred tool.

---

## Hybrid Attack

Hybrid attack = Dictionary + Bruteforce.

Example:
```
password123  
password121  
```

Tools add variations like numbers/symbols to known dictionary words.

Use `John the Ripper` or `Hashcat` in hybrid mode.

---

## Credential Stuffing

Attackers use leaked credentials from one platform to try on others.

Example:
```
Email: abc@abc.com  
Password: abc123  
```

If this combo leaks from one site, the attacker tries it on:

- Instagram  
- Facebook  
- Twitter  
- Gmail  

Tools often used: `SentryMBA`, `Snipr`, `OpenBullet`.

---

## Rainbow Table Attack

A **rainbow table** is a pre-computed list of hash values mapped to original words.

Example:

```
MD5: 5d41402abc4b2a76b9719d911017c592  
Plaintext: hello
```

Attacker compares the target hash with this list.

### Create your own hash:
```bash
echo -n "hello" | md5sum
```

Tools:
- [CrackStation](https://crackstation.net/)
- `RainbowCrack`
- `Hashcat`

---

## Conclusion

Understanding these attacks helps you protect systems:

- Always use strong, random passwords  
- Enable two-factor authentication (2FA)  
- Monitor for credential leaks  
- Use a password manager  

Stay safe and keep learning cybersecurity ethically!

---


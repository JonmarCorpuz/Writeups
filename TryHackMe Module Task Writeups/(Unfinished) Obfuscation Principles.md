# Room Information

![]()

Room link: https://tryhackme.com/room/obfuscationprinciples

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 08/16/2023

# Room Tasks

## Object Concatenation

1. Started up this task's machine
2. Opened up Notepad to create our script containing our obfuscated code and save it onto the machine
![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Object%20Concatenation%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Object%20Concatenation%20pt2.png)

3. We'll go to <TARGET IP> and upload the obfuscated script, which ended up going through and getting executed, which ended up giving us the flag for this task
![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Object%20Concatenation%20pt3.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Object%20Concatenation%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Object%20Concatenation%20pt5.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Object%20Concatenation%20pt6.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Object%20Concatenation%20pt7.png)


**TASK COMPLETED!**

## Arbitrary Control Flow Pattern

1. Followed the logic of the provided Python code to obtain this task's flag
```Python
x = 3
swVar = 1
a = 112340857612345
b = 1122135047612359087
i = 0
case_1 = ["T","d","4","3","3","3","e","1","g","w","p","y","8","4"]
case_2 = ["1a","H","3a","4a","5a","3","7a","8a","d","10a","11a","12a","!","14a"]
case_3 = ["1b","2b","M","4b","5b","6b","c","8b","9b","3","11b","12b","13b","14b"]
case_4 = ["1c","2c","3c","{","5c","6c","7c","8c","9c","10c","d","12c","13c","14c"]
case_5 = ["1d","2d","3d","4d","D","6d","7d","o","9d","10d","11d","!","13d","14d"]
case_6 = ["1e","2e","3e","4e","5e","6e","7e","8e","9e","10e","11e","12e","13e","}"]

while (x > 1):
    if (x % 2 == 1):
        x = x * 3 + 1
    else:
        x = x / 2
    if (x == 1):
        for y in case_1:
            match swVar:
                case 1:
                    print(case_1[i])
                    a = 2
                    b = 214025
                    swVar = 2
                case 2:
                    print(case_2[i])
                    if (a > 10):
                        swVar = 6
                    else:
                        swVar = 3
                case 3:
                    print(case_3[i])
                    b = b + a
                    if (b < 10):
                        swVar = 5
                    else:
                        swVar = 4
                case 4:
                    print(case_4[i])
                    b -= b
                    swVar = 5
                case 5:
                    print(case_5[i])
                    a += a
                    swVar = 2
                case 6:
                    print(case_5[11])
                    print(case_6[i])
                    break
            i = i + 1 
```

## Protecting and Stripping Identifiable Information

1. Started this task's machine
2. `mousepad <FILENAME>` to open up Mousepad, which is where we'll copy paste, and then obfuscate the provided C++ code by removing any meaningful identifiers by renaming them into meaningless identifiers, as well as strip any code, header files, or debug information that isn't necessary for the script to perform what it's supposed to perform
`mousepad challenge-8.exe`
```Bash
root@ip-10-10-122-116:~# mousepad challenge-8.exe
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Protecting%20and%20Stripping%20Identifiable%20Information%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Protecting%20and%20Stripping%20Identifiable%20Information%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Protecting%20and%20Stripping%20Identifiable%20Information%20pt3.png)

3. After removing meaningful identifiers and stripping unnecessary information from the provided C++ code, we went to <TARGET IP> and uploaded the obfuscated script, which ended up giving us this task's flag
`firefox "<TARGET IP>"`
```Bash
root@ip-10-10-122-116:~# firefox 10.10.54.16
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Protecting%20and%20Stripping%20Identifiable%20Information%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Protecting%20and%20Stripping%20Identifiable%20Information%20pt5.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Obfuscation%20Principles/Assets/Protecting%20and%20Stripping%20Identifiable%20Information%20pt6.png)


**TASK COMPLETED!**

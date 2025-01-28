# Challenge Name: Packet Cyclone

## Challenge Description

Pandora's friend and partner, Wade, leads the investigation into the relic's location. Recently, he noticed some weird traffic coming from his host. That led him to believe that his host was compromised. After a quick investigation, his fears were confirmed. Pandora now tries to determine if the attacker caused the suspicious traffic during the exfiltration phase. Pandora believes that the malicious actor used **rclone** to exfiltrate Wade's research to the cloud. Using the tool called "chainsaw" and the sigma rules provided, can you detect the usage of rclone from the event logs produced by Sysmon? To get the flag, you need to start and connect to the Docker service and answer all the questions correctly.

To analyze the logs file, they mention the tool name `chainsaw`.

Chainsaw is a tool to analyze event logs. Learn more about Chainsaw [here](https://github.com/WithSecureLabs/chainsaw.git).

Use Chainsaw's hunt mode to analyze logs by using the command:

```bash
./chainsaw hunt {path/to/Logs/} -s {path/to/sigma_rules/} --mapping chainsaw/mappings/sigma-event-logs-all.yml -r {path/to/sigma_rules/}
```

![Screenshot from 2025-01-28 12-01-46](https://github.com/user-attachments/assets/08e11d95-fc7d-4df7-9db9-54843d5f477a)

Now start your instance. They provide some questions based on the logs file.

![Screenshot from 2025-01-28 12-20-47](https://github.com/user-attachments/assets/d876f49b-72da-4676-9a6e-922f4b597b2e)

### Questions and Answers

1. **What is the email of the attacker used for the exfiltration process?**
   - **Answer:** `majmeret@protonmail.com`

   ![Screenshot from 2025-01-28 12-21-10](https://github.com/user-attachments/assets/3b7584c5-a8ee-4c68-a9ab-6e78881dd1f0)

2. **What is the password of the attacker used for the exfiltration process?**
   - **Answer:** `FBMeavdiaFZbWzpMqIVhJCGXZ5XXZI1qsU3EjhoKQw0rEoQqHyI`

3. **What is the Cloud storage provider used by the attacker?**
   - **Answer:** `mega`

   ![Screenshot from 2025-01-28 12-23-32](https://github.com/user-attachments/assets/9f689007-873e-40de-a239-28c31505b07a)

4. **What is the ID of the process used by the attackers to configure their tool?**
   - **Answer:** `3820`

5. **What is the name of the folder the attacker exfiltrated? Provide the full path.**
   - **Answer:** `C:\Users\Wade\Desktop\Relic_location`

6. **What is the name of the folder the attacker exfiltrated the files to?**
   - **Answer:** `exfiltration`

### Flag

```HTB{Rcl0n3_1s_n0t_s0_inn0c3nt_4ft3r_4ll}```


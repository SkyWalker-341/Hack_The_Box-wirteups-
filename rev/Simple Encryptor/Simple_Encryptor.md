# Simple Encryptor

## Description
During a routine check of our secret flag storage server, we discovered that it had been compromised by ransomware! The original flag data is missing, but fortunately, we still have both the encrypted file and the encryption program used in the attack.

Upon analyzing the ELF encryption program using Ghidra, we examined the `main` function and discovered the encryption process:

- The program reads the flag file into memory.
- It seeds the random number generator with the current timestamp.
- The encryption process consists of:
  - XOR operation with a random value.
  - Bitwise left-rotation by a random number of bits.
- The program writes the encryption seed (timestamp) and the encrypted data into `flag.enc`.
- Stack protection mechanisms are in place to prevent buffer overflow attacks.

![Screenshot from 2025-03-13 17-29-07](https://github.com/user-attachments/assets/e1299480-ffe7-41bd-a588-3d5199b3b58a)


### Decryption Process
To recover the original flag, we reverse the encryption process:
- Extract the timestamp from `flag.enc` (first 4 bytes).
- Use this timestamp to reseed the random number generator (`srand(seed)`).
- Read the encrypted data and apply the inverse operations:
  - Perform a bitwise right-rotation (instead of left-rotation).
  - XOR with the same sequence of random values.

script : 
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main()
{


 FILE *fp = fopen("flag.enc", "rb");         
 
 if (fp != NULL)
 {

  fseek(fp, 0, SEEK_END);
  long size = ftell(fp);
  rewind(fp);

  char *fileContents = malloc(size);
  
  if (fileContents != NULL)
  {


   fread(fileContents, sizeof(char), size, fp);
   
   for (int i = 0; i < size; i++)
   {
    printf("%02X", fileContents[i]);
   }
   printf("\n\n");

   int seed;
   memcpy(&seed, fileContents, sizeof(seed));
   printf("Seed: %d\n", seed);

   srand(seed);
   int rand1, rand2;

   for (int i = 4; i < size; i++)
   {
    rand1 = rand();
    rand2 = rand() & 7;

    printf("current byte: %02X\n", fileContents[i]);
    printf("right shift: %d\n", rand2);
    printf("XOR key: %d\n", rand1);

    fileContents[i]  = ((unsigned char)fileContents[i] >> (rand2)) | ((fileContents[i]) << (8 - rand2));
    printf("byte after rotate right: %02X\n", fileContents[i]);

    fileContents[i] = rand1 ^ fileContents[i];
    
    printf("byte after full decryption: %02X\n", fileContents[i]);
    getchar();   
   }

   for (int i = 4; i < size; i++)
   {
    printf("%c", fileContents[i]);
   }
  
  }
  
  free(fileContents);
  fclose(fp); 
 }

return 0;
}
```

### Flag : HTB{vRy_s1MplE_F1LE3nCryp0r}
---


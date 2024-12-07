# Trivial Flag Transfer Protocol
## Problem
- Figure out how they moved the flag.
## Thought Process
- `.pcapng` so I opened it with WireShark. 
- Exported Objects as TFTP
- Now I have these Files
![image](https://github.com/user-attachments/assets/42dc766b-061c-483c-9c76-efa438b1694d)
- `instructions.txt` looks like it is encrypted, so I used a simple ROT13 online.
![image](https://github.com/user-attachments/assets/2a4d67b8-5ea9-4956-bdd0-7528be3c371c)
- `TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN`
- This suggests the plan might contain something, so I opened the plan file using vim.
![image](https://github.com/user-attachments/assets/251d877a-144b-462f-8991-3f52a4e73c46)
- Time to check out the `.bmp` files.
- The program contains steghide
- So we use steghide to decrypt the data from the 3 pictures
- The dash between with and duediligence suggests that duediligence is the passphrase
- Didn't work at first, so I tried it in all caps, as the cipher decoded it.
```console
steghide extract -sf picture3.bmp -p DUEDILIGENCE
```
- ![image](https://github.com/user-attachments/assets/2689bb0c-c813-4618-ac49-57c3057f7d7e)
- Time to read the flag.
- Flag: `picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}`
# Tunn3l V1s10n
## Problem
- We found this file. Recover the flag.
## Thought Process
- Tried opening the file, big mistake, crashed my WSL.
- Opened it with a hex editor
![image](https://github.com/user-attachments/assets/fdda6f81-f30f-418d-ae57-48b7b0fb326f)
- BM suggests that it is a bitmap file
- Saving it as a `.bmp` file, expecting it to be openable now.
- Of course, it still cannot be opened. Probably contains further corruption in the file header.
- `BA D0` is probably the corrupted hex.
- Seeing as the number of bytes in the DIB header are wrong and the number of bits per pixel are also wrong we realize we need to edit the bytes at offset 0x0e and 0x1c. 
  

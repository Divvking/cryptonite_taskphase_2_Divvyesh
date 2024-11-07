# C3
## Problem
Some Custom Cyclical Cipher has been provided, we need to obtain the flag
## Thought Process
- Step 1, try to understand the encryption method
```python
import sys
chars = ""
from fileinput import input
for line in input():
  chars += line
lookup1 = "\n \"#()*+/1:=[]abcdefghijklmnopqrstuvwxyz"
lookup2 = "ABCDEFGHIJKLMNOPQRSTabcdefghijklmnopqrst"
out = ""
prev = 0
for char in chars:
  cur = lookup1.index(char)
  out += lookup2[(cur - prev) % 40]
  prev = cur
sys.stdout.write(out)
```
- `(cur - prev) % 40` gives the difference between the current and previous indices, wrapped to 40
- Reversing should be simple and just need to swap `lookup1` and `lookup2`
```python
# decryption code
ciphertext = "DLSeGAGDgBNJDQJDCFSFnRBIDjgHoDFCFtHDgJpiHtGDmMAQFnRBJKkBAsTMrsPSDDnEFCFtIbEDtDCIbFCFtHTJDKerFldbFObFCFtLBFkBAAAPFnRBJGEkerFlcPgKkImHnIlATJDKbTbFOkdNnsgbnJRMFnRBNAFkBAAAbrcbTKAkOgFpOgFpOpkBAAAAAAAiClFGIPFnRBaKliCgClFGtIBAAAAAAAOgGEkImHnIl"
lookup1 = "\n \"#()*+/1:=[]abcdefghijklmnopqrstuvwxyz"
lookup2 = "ABCDEFGHIJKLMNOPQRSTabcdefghijklmnopqrst"
out = ""
prev = 0
for char in ciphertext:
  cur = lookup2.index(char)
  original_index = (cur + prev) % 40
  out += lookup1[original_index]
  prev = original_index

print(out)


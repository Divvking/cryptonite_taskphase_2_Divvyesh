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
```
- On running, the following code is obtained
```python
#asciiorder
#fortychars
#selfinput
#pythontwo

chars = ""
from fileinput import input
for line in input():
    chars += line
b = 1 / 1

for i in range(len(chars)):
    if i == b * b * b:
        print chars[i] #prints
        b += 1 / 1
```
- it says #pythontwo, so using that version, `1/1` gives `1.0`.
- Program basically checks for perfect cube then displays output
- Since I couldn't get python2 to install, I rewrote it for python3
- it says `#selfinput` so I ran the program as input.
- flag: picoCTF{adlibs}
# Custom Encryption
## Problem
- Can you get sense of this code file and write the function that will decode the given encrypted file content.
## Thought Process
- First I checked the encrypted flag and tried to make sense of the encryption method.
- I will be adding my comprehension of the code in the form of comments in the code block itself
```python
from random import randint
import sys


def generator(g, x, p): #computes g^x%p
    return pow(g, x) % p


def encrypt(plaintext, key): #ascii*key*311
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher


def is_prime(p):#checks for prime
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True

# Encrypts plaintext by XORing each reversed char with characters from text_key
def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text

#main encryption test function
def test(plain_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    #generate random numbers close to p and q
    a = randint(p-10, p)
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    #check for shared key
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    #encryption using the dynamic xor
    semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
    cipher = encrypt(semi_cipher, shared_key)
    print(f'cipher is: {cipher}')

#Entry point of the program
if __name__ == "__main__":
    message = sys.argv[1]
    test(message, "trudeau")
```
- To reverse it, I did the following code
```python
from random import randint
import sys


def generator(g, x, p):
    return pow(g, x) % p


def decrypt(ciphertext, key):
    plain = []
    for num in ciphertext:
        plain+=chr(int((num/key/311)))#take ascii product, divide by key and 311, then convert to char and append to plaintext 
    return plain


def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True


def dynamic_xor_decrypt(ciphertext, text_key):
    plain_text = ""
    key_length = len(text_key)
    for i, char in enumerate(ciphertext):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        plain_text += decrypted_char
    return plain_text


def test(cipher_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = 90 #randint(p-10, p)
    b = 26 #randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    semi_cipher = decrypt(cipher_text, shared_key)
    plain = dynamic_xor_decrypt(semi_cipher, text_key)
    print(f'plain is: {plain}')


if __name__ == "__main__":
    message = [61578, 109472, 437888, 6842, 0, 20526, 129998, 526834, 478940, 287364, 0, 567886, 143682, 34210, 465256, 0, 150524, 588412, 6842, 424204, 164208, 184734, 41052, 41052, 116314, 41052, 177892, 348942, 218944, 335258, 177892, 47894, 82104, 116314]
    test(message, "trudeau")
```
- Basically, I just reversed the order of execution and the functions' logic as well. It took some time but I managed to obtain a result, just that it came in reverse, so I just reversed it using an online tool, even though it would take just a few characters on the script.
```bash
a = 90
b = 26
plain is: }b5eebf94_d6tp0rc2d_motsuc{FTCocip
```
- flag: picoCTF{custom_d2cr0pt6d_49fbee5b}
# miniRSA
## Problem
- We have a ciphertext file, and the problem name suggests the use of RSA
## Thought Process
- RSA is a public key cryptosystem, meaning that messages are encrypted with a public key that is known to everyone and decrypted using a private key known only to the receiver. This private key and public key are generated ahead of time. The generation uses primes, since the time needed to factorize a large number into its prime factors is infeasibly high for large numbers and large primes.
- reading the file, we have
```
N: 29331922499794985782735976045591164936683059380558950386560160105740343201513369939006307531165922708949619162698623675349030430859547825708994708321803705309459438099340427770580064400911431856656901982789948285309956111848686906152664473350940486507451771223435835260168971210087470894448460745593956840586530527915802541450092946574694809584880896601317519794442862977471129319781313161842056501715040555964011899589002863730868679527184420789010551475067862907739054966183120621407246398518098981106431219207697870293412176440482900183550467375190239898455201170831410460483829448603477361305838743852756938687673
e: 3

ciphertext (c): 2205316413931134031074603746928247799030155221252519872650080519263755075355825243327515211479747536697517688468095325517209911688684309894900992899707504087647575997847717180766377832435022794675332132906451858990782325436498952049751141
```
- `N=p*q` where p and q are prime numbers
- `totient(n) = (p - 1) * (q - 1)`
- Pick an `e` between 1 and `totient(n)` so that totient(n) and e share no common factors. e is often `65537` due to its efficiency. This, in combination with n, is the public key.
- To decrypt a message c, find pow(c,d,n).
- Since the exponent is small, plaintext m is small enough that `m^e<N`
- ciphertext c's cube root will simply give the plaintext
- attempting to find cuberoot with maximum accuracy using binary search
```python
def find_invpow(x,n):
    """Finds the integer component of the n'th root of x,
    an integer such that y ** n <= x < (y + 1) ** n.
    """
    high = 1
    while high ** n < x:
        high *= 2
    low = high/2
    while low < high:
        mid = (low + high) // 2
        if low < mid and mid**n < x:
            low = mid
        elif high > mid and mid**n > x:
            high = mid
        else:
            return mid
    return mid + 1
print (find_invpow(2205316413931134031074603746928247799030155221252519872650080519263755075355825243327515211479747536697517688468095325517209911688684309894900992899707504087647575997847717180766377832435022794675332132906451858990782325436498952049751141,3))
```
result: `Plaintext (m): 13016382529449106065894479374027604750406953699090365388203722801043052339225981`
- converting to hex then ascii
```python
def number_to_ascii(number):
    hex_representation = hex(number)[2:]  # Convert to hex and remove '0x'
    if len(hex_representation) % 2 != 0:
        hex_representation = '0' + hex_representation
    return bytes.fromhex(hex_representation).decode('utf-8', errors='ignore')
m = 13016382529449106065894479374027604750406953699090365388203722801043052339225981

# Decode the plaintext to ASCII
plaintext_message = number_to_ascii(m)
print(f"Decoded Plaintext: {plaintext_message}")
```
- flag is `picoCTF{n33d_a_lArg3r_e_d0cd6eae}`

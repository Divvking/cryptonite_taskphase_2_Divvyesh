# freakada 3301
- This level seemed like a proper part by part imitation of the first puzzle of Cicada 3301
![image](https://github.com/user-attachments/assets/dad6c739-648e-4b21-82be-ce16664afb8d)
- Uploading the image on aperisolve, I found the following which was interesting,
![image](https://github.com/user-attachments/assets/0eff7772-e8cb-4ada-b90a-aec4147ba0ad)
- Using a monoalphabetic substituition decoder, I obtained `THE PATH IS OBSCURED, BUT THE TRUTH LIES IN THE SHADOWS. SEEK THE PATTERNS. AMONG THE CHAOS, A BEACON AWAITS: HTTPS://BIT.LY/FREAKESTEIN0. ONLY THE WORTHY WILL SEE THE UNSEEN. THE JOURNEY HAS JUST BEGUN`
- This leads to another jpg file, in a protondrive link
![image](https://github.com/user-attachments/assets/7ca365d8-a44e-45c8-a5b8-105b0c5f64ec)
- This image contains a secret.txt which in turn has a link - `https://github.com/freakada-3301`
![image](https://github.com/user-attachments/assets/9378ead3-2055-4c15-ad5e-5ca6868c8e20)
- README.md contains `1sk3vr3d_qw5yvvc}` which is the 2nd part of the flag, possibly encoded with a vigenere key
- Opening the commit history, we obtain a poem inside `Whispers of the forgotten` and a key in `Code of decay`
https://github.com/freakada-3301/Salvation-in-Decay/commit/8951d1e8fc35e2a4845ee8a740ac343ce85420ee
https://github.com/freakada-3301/Salvation-in-Decay/commit/618ff2a8e6a8b724ad2ad909c52ed7bd798ee6b0
- The code is in `line:word:character` format. Taking the corresponding characters, we get a link to a discord server : `https://discord.gg/AHj7R2Qd`
![image](https://github.com/user-attachments/assets/092cd6ad-ecf9-4ac3-94cd-ce8fce0614a5)
- Nice bait, all it does is add to the gen alpha humour by playing KSI's classic `Thick of It ft. Trippie Redd`. There's a freakada bot which we are supposed to dm.
![image](https://github.com/user-attachments/assets/0f90cf7b-2314-45b0-b794-e25d4ad2a7e1)
- Tried a bunch of brainrot DMs to the bot, which resulted in the bot responding `INCORRECT`
- Then I decided to message 3301, which is a prime number (this was random DM luck, I realised the reason only later).
![image](https://github.com/user-attachments/assets/6e620409-f996-4ad8-b6c1-9b955046032a)
- x and y are the dimensions of the original image, so `3301*449*503` i.e. `745520947`
![image](https://github.com/user-attachments/assets/f9024062-f02d-4bc1-bcb3-ebd81dcc68c0)
- The coordinates lead to a location called `Freaky Backshots Cliff`!?!?! On the Manipal Institute of Technology campus. Going to the Google images provides a picture of a freakada 3301 poster with a QR Code on it. It leads to a protondrive containing a `.wav` file. Using a spectrogram, we obtain the first part of the flag `nite{4_wannabe_` and some morse
- `.._. ._. . . _._ . _.__` is the morse which translates to `FREEKEY`, using it as a key for the 2nd part of the flag
flag: `nite{4_wannabe_1nt3rn3t_my5tery}`
# la casa de papel
- They've given us some stuff, didn't need to bother with any of that.
- Ran the Practice Convo, Obtained the b64 response for Bob, decoded that to get the HMAC
- Ran the 2nd option to fool the bot. Submitted the HMAC, then used the password on the 3rd option to get flag.
```console
root@LAPTOP-D1CLF8OQ:~# ncat --ssl la-casa-de-papel.chals.nitectf2024.live 1337
 _             ____                      _        ____                  _
| |    __ _   / ___|__ _ ___  __ _    __| | ___  |  _ \ __ _ _ __   ___| |
| |   / _` | | |   / _` / __|/ _` |  / _` |/ _ \ | |_) / _` | '_ \ / _ \ |
| |__| (_| | | |__| (_| \__ \ (_| | | (_| |  __/ |  __/ (_| | |_) |  __/ |
|_____\__,_|  \____\__,_|___/\__,_|  \__,_|\___| |_|   \__,_| .__/ \___|_|
                                                            |_|


1. Practice Convo
2. Let's Fool Alice!
3. Crack the Vault
4. Exit
Choose an option: 1
Send a message: Bob
Here is your encrypted message: YjRlMGE4MDI0MjhjYjM1ZjY5YzBlOTUyZDk2MTcyZDY=

1. Practice Convo
2. Let's Fool Alice!
3. Crack the Vault
4. Exit
Choose an option: 2

Bot: Okay, let's see if you're the real deal. What's your name?
Your name: Bob

Bot: Please provide your HMAC
Your HMAC: b4e0a802428cb35f69c0e952d96172d6

Alice: Oh hey Bob! Here is the vault code you wanted:
G0t_Th3_G0ld_B3rl1nale

1. Practice Convo
2. Let's Fool Alice!
3. Crack the Vault
4. Exit
Choose an option: 3

Vault Person: Enter password
Password: G0t_Th3_G0ld_B3rl1nale

Vault Unlocked! The flag is: nite{El_Pr0f3_0f_Prec1s10n_Pl4ns}
```
# R Stands Alone
```python3
from Crypto.Util.number import *

def gen_keys():
    while True:
        a = getPrime(128)
        b = getPrime(128)
        A = a+b
        B = a-b 
        
        p = ((17*A*A*A) - (15*B*B*B) - (45*A*A*B) + (51*A*B*B)) // 8

        if isPrime(p) :
            return a, b, p
    
p, q, r = gen_keys()
e = 65537
n = p*q*r

flag = b"nite{REDACTED}"

ct = pow(bytes_to_long(flag), e, n)
print(f"{r =}")
print(f"{ct =}")

"""OUTPUT :
r =17089720847522532186100904495372954796086523439343401190123572243129905753474678094845069878902485935983903151003792259885100719816542256646921114782358850654669422154056281086124314106159053995410679203972646861293990837092569959353563829625357193304859110289832087486433404114502776367901058316568043039359702726129176232071380909102959487599545443427656477659826199871583221432635475944633756787715120625352578949312795012083097635951710463898749012187679742033
ct =583923134770560329725969597854974954817875793223201855918544947864454662723867635785399659016709076642873878052382188776671557362982072671970362761186980877612369359390225243415378728776179883524295537607691571827283702387054497203051018081864728864347679606523298343320899830775463739426749812898275755128789910670953110189932506526059469355433776101712047677552367319451519452937737920833262802366767252338882535122186363375773646527797807010023406069837153015954208184298026280412545487298238972141277859462877659870292921806358086551087265080944696281740241711972141761164084554737925380675988550525333416462830465453346649622004827486255797343201397171878952840759670675361040051881542149839523371605515944524102331865520667005772313885253113470374005334182380501000
"""
```
- r and the ciphertext are provided
- `ct=(flag)**e %n`
- `phi(r) = r-1`
```python3
from Crypto.Util.number import *

# Given values
r = 17089720847522532186100904495372954796086523439343401190123572243129905753474678094845069878902485935983903151003792259885100719816542256646921114782358850654669422154056281086124314106159053995410679203972646861293990837092569959353563829625357193304859110289832087486433404114502776367901058316568043039359702726129176232071380909102959487599545443427656477659826199871583221432635475944633756787715120625352578949312795012083097635951710463898749012187679742033
ct = 583923134770560329725969597854974954817875793223201855918544947864454662723867635785399659016709076642873878052382188776671557362982072671970362761186980877612369359390225243415378728776179883524295537607691571827283702387054497203051018081864728864347679606523298343320899830775463739426749812898275755128789910670953110189932506526059469355433776101712047677552367319451519452937737920833262802366767252338882535122186363375773646527797807010023406069837153015954208184298026280412545487298238972141277859462877659870292921806358086551087265080944696281740241711972141761164084554737925380675988550525333416462830465453346649622004827486255797343201397171878952840759670675361040051881542149839523371605515944524102331865520667005772313885253113470374005334182380501000
e = 65537
# Compute the modular inverse of e modulo (r-1)
d_r = inverse(e, r - 1)
# Decrypt the ciphertext modulo r
flag_r = pow(ct, d_r, r)

# Convert the decrypted value to bytes and decode
flag_bytes = long_to_bytes(flag_r)
flag = flag_bytes.decode()
# Output the recovered flag
print("Recovered flag:", flag)
```
```console
Recovered flag: nite{7h3_Latt1c3_kn0ws_Ur_Pr1m3s_very_vvery_v3Ry_w3LLL}
```

# Dachschund Attack Decryption in picoCTF

## Overview
This script demonstrates the use of **Wiener's attack** to recover the private key (`d`) from an RSA public key (`e`, `n`). If successful, it decrypts an encrypted message (`c`). Wiener's attack exploits **small private exponent vulnerabilities** in RSA encryption.

## Prerequisites
Ensure you have Python installed along with the required dependency:

```bash
pip install owiener
```

## How It Works
1. The script takes the RSA public key components (`e` and `n`) and an encrypted message (`c`).
2. It applies **Wiener's attack** using the `owiener` module to recover `d` if it is small.
3. If `d` is found, it decrypts `c` using RSA decryption (`M = c^d mod n`).
4. The decrypted message is converted from hexadecimal to plaintext.

## Code
```python
import owiener
import binascii

# RSA Public Key Components, this was simulated using the webshell in picoctf
e = 70210023413984271238440128307426004929485820198117173745052269060735736893148749273018243799148879990861128037595031367120748908915938351435530697527749321815064140703929446402876042770869569859313688559925438998220905118424228450691316997862619740851757907598088116482166419415576219061044832134775257983877
n = 120511527359095775585682808816939035249486805446553728341127357202432859033871428567285028746622773818800587826311163564454343382998957206470538343853013188011389089381128603270698213766815884688069101888333317044835146353060204067921049212563082614480354164132947972696704417750157985819932328441355672258763
c = 2358446415232376323393954209135054874236923724060419785085249532878028972794113807046118365394565342150874305346755209577033677476141830831153504406156687687717073216189172071173902957260492503539777400008210457574746952062437166012982762517524817548562813458504115341436645987502215301143350948305959883833

# Apply Wiener's Attack
d = owiener.attack(e, n)

if d is None:
    print("Failed")
else:
    print("Hacked d = {}".format(d))
    M = pow(c, d, n)
    print("Decrypted message: ", M)

    hex_m = format(M, 'x')  
    decryptm = binascii.unhexlify(hex_m)
    print("Decrypted message to plaintext: ", decryptm)
```

## Usage
Run the script using Python:

```bash
python dachshund.py
```

If the attack is successful, it will output the decrypted message.

## Notes
- Wiener's attack is effective **only if** `d` is **small** relative to `n`.
- If `d` is not found, the script will print **"Failed"**.
- This script is intended for **educational and research purposes only**. Do not use it for unauthorized activities.

## References
- **[Wiener's Attack on RSA](https://en.wikipedia.org/wiki/Wiener%27s_attack)**
- **[owiener Python Library](https://pypi.org/project/owiener/)**


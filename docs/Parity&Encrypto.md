# Verification & Encryption
## Verification
When information is tranmitted from the sender to the receiver, the receiver needs a method to check whether the information was changed during the transportation. Then we need to consider if we can recover the change. The same thing happens when information is stored somewhere like a disk, and after a period of time the information may get lost. So the problem is about three things: whether the information is changed, where the changes happened and how can we recover it.  

### Parity 
Odd-even Parity is the simpliest way of parity. Suppose you have a sequence of 1 and 0 to pass from the sender to the receiver. Then the sender can make a convention with the receiver: first, append one additional bit at the end of the sequence. And whether the last bit is 1/0 is dicided by the number of 1 in the sequence so that the number of 1 in the whole sequence should be even. And if the receiver found that there are odd number of 1 in the sequence, it should be affirm that something must be wrong in the trans mission.  

However only one additional bit can only tell the receiver whether the information is damaged, but not where the damage happens. So two-dimensional parity took place, and it can correct single-bit error in a sequence.  
> ### Two-dimensional Parity
>Suppose a single bit was changed in a sequence of *d* digits 1/0 sequence during transmission. To detect the position of the error, we may make a table like this:(suppose *d* = 15)  
>1 0 1 0 1 &emsp; 1  
>1 0 1 1 0 &emsp; 0  
>0 1 1 1 0 &emsp; 1  
>  
>0 0 1 0 1 &emsp; 0  
>The sender can fold the whole sequence into a 3*5 table, and for each row and column set one parity bit, so that the number of 1 in every row and column should be even. So we get an additional parity row and an additonal parity colomn. From the example shown, we can easily find the 0 at (2,2) should be 1.
This kind of ability to detect and correct errors is called Forward Error Correction(FEC). 

### Checksum


### CRC 


## Encryption
The need of encryption appeared from a long time ago, when people need to tranmit some message secretly. Different encryption methods are widely used in network technologies nowadays. The main encryption technologies are **Symmetric Encryption** and **Asymmetric Encryption**.  
### Symmetric Encryption
Caeser encryption is a typical symmetric encryption method. The sender and the receiver take a number *X* they all know before passing meassages. For every character in the message, the sender change it by the character *X* position after the original one in the alphabet. For instance, if they choose *X* = 2, then all 'a' in the message will be replaced by 'c',and 'b' will be replaced by 'd', and so on. When the receiver get the message, they should replace on the opposite direction, that is replace 'c' by 'a', 'd' by 'b', and so on.
So the sender and the receiver use the same way to encipher and decipher in symmetric encryption.  
However these encryption methods have some problems and are easy to hack. Since the *X* must be decided and known by both sender an receiver, but how should we pass the *X* secretly? And the hacker can easily guess the *X* since the pattern is not complicated.

### Asymmetric Encryption
To explain what 'asymmetric' mean here, let's use the caeser encryption example again. To make an analogy, it's like the sender replaces 'a' by 'c' to encipher, but the receiver uses 'c'by 'b' to decipher. But how does that even possible? Let's dive into 2 actual asymmetric encryption methods: RSA and ECC.  
#### RSA
RSA encryption is named after the name of the 3 inventers. It widely used in Internet(for example SSH protocal).  

##### Encipher and decipher process
First, the sender pick 2 prime number *p* and *q*, for example *p* = 61, *q* = 53. Then calculate *n* = *pq* and ϕ(*n*) = (*p*-1) * (*q*-1), in our example *n* = 3233 and ϕ(*n*) = 3120.  
Second, pick the **public key** (*e*,*n*)   
so that 1< *e* < ϕ(*n*) and gcd(*e*,ϕ(*n*))=1(which means *e* and ϕ(*n*) are relatively prime). In our example, suppose we choose *e* = 17.  
Next, calculate the **private key** (*d*,*n*)  
*d* should fit the formula: *e* * *d* ≡ 1(mod ϕ(*n*)), in our example we need to get *d* from 17 \* *d* ≡ 1(mod 3120), which is 2753(will be explained later).
Before passing message, both sides should generate their own public key:  
(*e<sub>1</sub>*,*n*), (*e<sub>2</sub>*,*n*)  
and private key: (*d<sub>1</sub>*,*n*), (*d<sub>2</sub>*,*n*).  
Then both sides should exchange their public key and keep their private key secret.  
To encipher message(let's say the number *X* = 65 is to be enciphered), the sender needs to calculate using the receiver's public key *e<sub>2</sub>*. And X<sup>*e<sub>2</sub>*</sup> mod n = *Y* (65<sup>17</sup> mod 3233 = 2790) is the final enciphered result.  
To decipher the result, the receiver need it's own private key *d<sub>2</sub>*. The deciphered result  
*Z* = *Y*<sup>*d<sub>2</sub>*</sup> mod *n* = 2790<sup>2753</sup> mod 3233 = 65 = *X*

##### Mathematical foundation
During the process of generating public key *e* and private key *d*, the concept of **modular inverse**. Specifically, *e* and *d* are the modular inverse about ϕ(*n*) of each other, fitting *e* \* *d* ≡ 1 (mod ϕ(n)).  
From   
*e* \* *d* ≡ 1 (mod ϕ(n))  
we can infer that  
*e* * *d* − 1 = k * ϕ(n)(k is some integer).  
So during encryption, *Y* = *X*<sup>*e*</sup> mod *n* and during decryption  
*Z* = *Y*<sup>*d*</sup> mod *n* = (*X*<sup>*e*</sup> mod *n*)<sup>*d*</sup> mod *n* = *X*<sup>*ed*</sup> mod *n*.  
Since *e* * *d* − 1 = k * ϕ(n),  
*X*<sup>*ed*</sup> = *X*<sup>1+kϕ(*n*) = *X* * (*X*<sup>ϕ(*n*)</sup>)<sup>k</sup>  
According to Euler's theory, if *X* and *n* are relatively prime,  
*X*<sup>ϕ(*n*)</sup> ≡ 1 (mod *n*).  
So *X*<sup>*ed*</sup> ≡ *X* * 1<sup>k</sup> ≡ *X* (mod *n*). Which means   
*Z* = *X*<sup>*ed*</sup> mod *n* =*X*

#### ECC

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
>We can fold the whole sequence into a 3*5 table, and for each row and column the sender set one parity bit, so that the number of 1 in every row and column should be even. From the example shown, we can easily find the 0 at (2,2) should be 1.  

This kind of ability to detect and correct errors is called Forward Error Correction(FEC). 

### Checksum


### CRC 


## Encryption
The need of encryption appeared from a long time ago, when people need to tranmit some message secretly. Different encryption methods are widely used in network technologies nowadays. The main encryption technologies are **Symmetric Encryption** and **Asymmetric Encryption**.  
### Symmetric Encryption
### Asymmetric Encryption
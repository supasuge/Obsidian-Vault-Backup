## Table of Contents

  - [The (mod p) operation](#The\(mod\p)\operation)
- [HAsty Pudding](#hasty\pudding)
- [Operations](#operations)


- Kyber - Key echange and encryption
- 8-round RC5 without any rotations. • 8-round RC5 with the rotation amount equal to the round number. • 12-round DES without any S-boxes. 
- 
-• 8 rounds of Skipjack’s rule B. (A description of Skipjack can be found on the World Wide Web.) 
• 4-round DES. • 
A generic cipher that is “closed” (i.e., encrypting with key A and then key B is the same as encrypting with key C, for all keys).
• 6-round DES. 
• 4 rounds of Skipjack’s rule A followed by four rounds of Skipjack’s rule B.

x³+1.x²+0x+1 = x³+x²+1

We can then multiply, divide, add and subtract polynomial values. If you remember from school, for (2x²+1) times (x+1) we get:

(2x²+1).(x+1) = 2x³+2x²+x+1

## The (mod p) operation

Obviously, if we are multiplying polynomials, the coefficients of the powers could become large, so we perform a (mod q) operation on them. Let’s take a q value of 17 and let’s say we have:

_a_=2_x_⁴+3_x_³+10_x_+3

_b_=3_x_⁵+14_x_³+10_x_+4

Then, to add (mod 17), we get:

_(2x_⁴+3_x_³+10_x_+3)_+(_3_x_⁵+14_x_³+10_x_+4) = _3x_⁵+2_x_⁴+17x³+20_x_+7  
= 3_x_⁵+2_x_⁴+3_x_+7 (mod 17)

To subtract (mod 17), we get:

_(2x_⁴+3_x_³+10_x_+3)-_(_3_x_⁵+14_x_³+10_x_+4) = -3x⁵+2x⁴-11x³-1  
= 14_x_⁵+2_x_⁴+6_x_³+16 (mod 17)

To multiply (mod 17), we get:

(2_x_⁴+3_x_³+10_x_+3)∗(3_x_⁵+14_x_³+10_x_+4)= [… working left to reader … ] =6_x_⁷+9_x_⁶+7_x_⁵+3_x_⁴+8_x_³+_x_²+2_x_+12 (mod17)

 Signal uses X3DH (“Extended Triple Diffie-Hellman”) and will migrate towards PQXDH (“Post-Quantum Extended Diffie-Hellman”). With PQXDH, we use a hybrid key exchange method of X25519 with CRYSTALS-Kyber. Overall, Kyber is the newly defined NIST standard for PQC key exchange.


# HAsty Pudding

![[imgs/img002.gif]]


![[imgs/img003.gif]]


# Operations
- Addition
- Subtration
- Exclusive-Or
- Shifts
==All 64-bit word, interpreted mor ^64


key expansion
subcipher has a keyexpansion table os 256 words, generated from the key

procedures of the kK table setup

1) init the key table
2) the first 128 words of key are  xor'd into the KX table (KX[0]-KX[127])
3) perform string function to pseudo-randomize the KX table
4) continue with step and 3 with the second 128 words of key until the entry key has bee xor'd into the key












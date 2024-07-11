## Table of Contents

          - [Source Code](#Source\Code)
      - [Understanding the Secret Sharing Scheme](#Understanding\the\Secret\Sharing\Scheme)
      - [LaGrange Interpolating Polynomials](#LaGrange\Interpolating\Polynomials)
      - [lolololoKek](#lolololoKek)
    - [Understanding the RSA Vulnerability](#Understanding\the\RSA\Vulnerability)
    - [The Challenge](#The\Challenge)
    - [Transition to the Integer Space: Howgrave-Graham's Theorem](#Transition\to\the\Integer\Space:\Howgrave-Graham's\Theorem)
    - [Implementing the Coppersmith Attack](#Implementing\the\Coppersmith\Attack)
    - [Python Demonstration with SageMath](#Python\Demonstration\with\SageMath)
    - [Key Points in the Code](#Key\Points\in\the\Code)
- [display matrix picture with 0 and X](#display\matrix\picture\with\0\and\x)
- [Test on Stereotyped Messages](#test\on\stereotyped\messages)
- [RSA gen options (for the demo)](#rsa\gen\options\(for\the\demo))
- [RSA gen (for the demo)](#rsa\gen\(for\the\demo))
- [Create problem (for the demo)](#create\problem\(for\the\demo))
- [Problem to equation (default)](#problem\to\equation\(default))
- [Tweak those](#tweak\those)
- [Coppersmith](#coppersmith)
- [output](#output)
- [Test on Factoring with High Bits Known](#test\on\factoring\with\high\bits\known)
- [RSA gen](#rsa\gen)
- [qbar is q + [hidden_size_random]](#qbar\is\q\+\[hidden_size_random])
- [PLAY WITH THOSE:](#play\with\those:)
- [Coppersmith](#coppersmith)
- [output](#output)




This took me way longer than I'd like to admit to finally get a response via telnet.....
i kept messing up the JSON response decoding lol.

###### Source Code
```python
I_forgot_to_copy_it_lol. Too lazy to find another writeup with it. smh. sry,
```
#### Understanding the Secret Sharing Scheme
Way to find a polynomial that passes through a given set of points (called nodes). Ex: ($x^j, y^i..$)
1. Polynomial construction: the secret key is the constant term of a polynomial of degree `d`. The other coefficients are random numbers.
2. **Share Generation**: A share is a point on this Polynomial. The `get_share` method generates this. you can request up to `n`

![[Pasted image 20231208211605.png]]
#### LaGrange Interpolating Polynomials
**Unique polynomial of lowest degree that interpolates a given set of data**
given Coodinate pairs
$(x^i,x^j)$ Such that $0 <= j <= k,$ the $x^j$ are called *nodes* and the $y^j$ are called the *values*

#### lolololoKek
`solution: {"command":"x":0}`

The Coppersmith attack is a fascinating aspect of cryptanalysis that targets the RSA encryption scheme. This attack is particularly intriguing because it exploits the mathematical properties of RSA to potentially uncover the private key, or in some cases, directly the plaintext from the ciphertext, under certain conditions. Here, we delve into the specifics of the Coppersmith attack, correct some of the mathematical inaccuracies, and present a clearer picture of how this attack operates, including a Python demonstration using SageMath, a powerful tool for algebraic number theory.

### Understanding the RSA Vulnerability

In RSA, encryption of a message mm is performed using a public key (N,e)(N,e), where NN is the modulus (a product of two large prime numbers, pp and qq), and ee is the public exponent. The encryption of mm is given by c=memod  Nc=memodN, where cc is the ciphertext.

The Coppersmith attack comes into play when the attacker wishes to find mm under certain conditions, particularly when mm can be expressed as m=m0+xm=m0​+x, where m0m0​ is known or guessed by the attacker, and xx is a small unknown value. The attacker forms a polynomial f(x)=c−(m0+x)emod  Nf(x)=c−(m0​+x)emodN, aiming to find the small root xx.

### The Challenge

The primary challenge in this scenario is that finding the roots of a polynomial modulo NN (when NN is a product of two large primes) is a difficult problem. Traditional algorithms for root finding do not directly apply here due to the modulo operation.

### Transition to the Integer Space: Howgrave-Graham's Theorem

To overcome this, a secondary polynomial g(x)g(x) is constructed, sharing the same roots as f(x)f(x) but defined over the integer space ZZ. The construction of g(x)g(x) leverages Howgrave-Graham's theorem, which provides conditions under which a small root of g(x)g(x) modulo NN can be found if it exists.

### Implementing the Coppersmith Attack

The attack employs the Lattice-Based Reduction technique, specifically the LLL (Lenstra-Lenstra-Lovász) algorithm, to find the small roots of g(x)g(x). This involves constructing a lattice from the coefficients of g(x)g(x) and applying the LLL algorithm to find a short vector in the lattice, which corresponds to a small root of the polynomial.

### Python Demonstration with SageMath

The provided Python code demonstrates the implementation of the Coppersmith attack using SageMath. It outlines the process of constructing the polynomial, forming the lattice, and applying the LLL algorithm to find the roots. The code is designed to be illustrative and demonstrates the attack in two scenarios: decrypting stereotyped messages and factoring with known high bits.

### Key Points in the Code

- **Initialization**: The polynomial and parameters for the attack are defined, considering the specific conditions of the RSA setup being targeted.
- **Optimization**: Parameters such as beta (ββ), epsilon (ϵϵ), and the bounds for XX are optimized based on the degree of the polynomial and the modulus NN.
- **Application of LLL**: The LLL algorithm is applied to the lattice constructed from the polynomial, aiming to find a short vector that reveals the small root.
```python
from __future__ import print_function
import time

debug = True

# display matrix picture with 0 and X
def matrix_overview(BB, bound):
    for ii in range(BB.dimensions()[0]):
        a = ('%02d ' % ii)
        for jj in range(BB.dimensions()[1]):
            a += '0' if BB[ii,jj] == 0 else 'X'
            a += ' '
        if BB[ii, ii] >= bound:
            a += '~'
        print(a)

def coppersmith_howgrave_univariate(pol, modulus, beta, mm, tt, XX):
    """
    Coppersmith revisited by Howgrave-Graham
    
    finds a solution if:
    * b|modulus, b >= modulus^beta , 0 < beta <= 1
    * |x| < XX
    """
    #
    # init
    #
    dd = pol.degree()
    nn = dd * mm + tt

    #
    # checks
    #
    if not 0 < beta <= 1:
        raise ValueError("beta should belongs in (0, 1]")

    if not pol.is_monic():
        raise ArithmeticError("Polynomial must be monic.")

    #
    # calculate bounds and display them
    #
    """
    * we want to find g(x) such that ||g(xX)|| <= b^m / sqrt(n)

    * we know LLL will give us a short vector v such that:
    ||v|| <= 2^((n - 1)/4) * det(L)^(1/n)

    * we will use that vector as a coefficient vector for our g(x)
    
    * so we want to satisfy:
    2^((n - 1)/4) * det(L)^(1/n) < N^(beta*m) / sqrt(n)
    
    so we can obtain ||v|| < N^(beta*m) / sqrt(n) <= b^m / sqrt(n)
    (it's important to use N because we might not know b)
    """
    if debug:
        # t optimized?
        print("\n# Optimized t?\n")
        print("we want X^(n-1) < N^(beta*m) so that each vector is helpful")
        cond1 = RR(XX^(nn-1))
        print("* X^(n-1) = ", cond1)
        cond2 = pow(modulus, beta*mm)
        print("* N^(beta*m) = ", cond2)
        print("* X^(n-1) < N^(beta*m) \n-> GOOD" if cond1 < cond2 else "* X^(n-1) >= N^(beta*m) \n-> NOT GOOD")
        
        # bound for X
        print("\n# X bound respected?\n")
        print("we want X <= N^(((2*beta*m)/(n-1)) - ((delta*m*(m+1))/(n*(n-1)))) / 2 = M")
        print("* X =", XX)
        cond2 = RR(modulus^(((2*beta*mm)/(nn-1)) - ((dd*mm*(mm+1))/(nn*(nn-1)))) / 2)
        print("* M =", cond2)
        print("* X <= M \n-> GOOD" if XX <= cond2 else "* X > M \n-> NOT GOOD")

        # solution possible?
        print("\n# Solutions possible?\n")
        detL = RR(modulus^(dd * mm * (mm + 1) / 2) * XX^(nn * (nn - 1) / 2))
        print("we can find a solution if 2^((n - 1)/4) * det(L)^(1/n) < N^(beta*m) / sqrt(n)")
        cond1 = RR(2^((nn - 1)/4) * detL^(1/nn))
        print("* 2^((n - 1)/4) * det(L)^(1/n) = ", cond1)
        cond2 = RR(modulus^(beta*mm) / sqrt(nn))
        print("* N^(beta*m) / sqrt(n) = ", cond2)
        print("* 2^((n - 1)/4) * det(L)^(1/n) < N^(beta*m) / sqrt(n) \n-> SOLUTION WILL BE FOUND" if cond1 < cond2 else "* 2^((n - 1)/4) * det(L)^(1/n) >= N^(beta*m) / sqroot(n) \n-> NO SOLUTIONS MIGHT BE FOUND (but we never know)")

        # warning about X
        print("\n# Note that no solutions will be found _for sure_ if you don't respect:\n* |root| < X \n* b >= modulus^beta\n")
    
    #
    # Coppersmith revisited algo for univariate
    #

    # change ring of pol and x
    polZ = pol.change_ring(ZZ)
    x = polZ.parent().gen()

    # compute polynomials
    gg = []
    for ii in range(mm):
        for jj in range(dd):
            gg.append((x * XX)**jj * modulus**(mm - ii) * polZ(x * XX)**ii)
    for ii in range(tt):
        gg.append((x * XX)**ii * polZ(x * XX)**mm)
    
    # construct lattice B
    BB = Matrix(ZZ, nn)

    for ii in range(nn):
        for jj in range(ii+1):
            BB[ii, jj] = gg[ii][jj]

    # display basis matrix
    if debug:
        matrix_overview(BB, modulus^mm)

    # LLL
    BB = BB.LLL()

    # transform shortest vector in polynomial    
    new_pol = 0
    for ii in range(nn):
        new_pol += x**ii * BB[0, ii] / XX**ii

    # factor polynomial
    potential_roots = new_pol.roots()
    print("potential roots:", potential_roots)

    # test roots
    roots = []
    for root in potential_roots:
        if root[0].is_integer():
            result = polZ(ZZ(root[0]))
            if gcd(modulus, result) >= modulus^beta:
                roots.append(ZZ(root[0]))

    # 
    return roots

############################################
# Test on Stereotyped Messages
##########################################    

print("//////////////////////////////////")
print("// TEST 1")
print("////////////////////////////////")

# RSA gen options (for the demo)
length_N = 1024  # size of the modulus
Kbits = 200      # size of the root
e = 3

# RSA gen (for the demo)
p = next_prime(2^int(round(length_N/2)))
q = next_prime(p)
N = p*q
ZmodN = Zmod(N);

# Create problem (for the demo)
K = ZZ.random_element(0, 2^Kbits)
Kdigits = K.digits(2)
M = [0]*Kbits + [1]*(length_N-Kbits); 
for i in range(len(Kdigits)):
    M[i] = Kdigits[i]
M = ZZ(M, 2)
C = ZmodN(M)^e

# Problem to equation (default)
P.<x> = PolynomialRing(ZmodN) #, implementation='NTL')
pol = (2^length_N - 2^Kbits + x)^e - C
dd = pol.degree()

# Tweak those
beta = 1                                # b = N
epsilon = beta / 7                      # <= beta / 7
mm = ceil(beta**2 / (dd * epsilon))     # optimized value
tt = floor(dd * mm * ((1/beta) - 1))    # optimized value
XX = ceil(N**((beta**2/dd) - epsilon))  # optimized value

# Coppersmith
start_time = time.time()
roots = coppersmith_howgrave_univariate(pol, N, beta, mm, tt, XX)

# output
print("\n# Solutions")
print("we want to find:",str(K))
print("we found:", str(roots))
print(("in: %s seconds " % (time.time() - start_time)))
print("\n")

############################################
# Test on Factoring with High Bits Known
##########################################
print("//////////////////////////////////")
print("// TEST 2")
print("////////////////////////////////")

# RSA gen
length_N = 1024;
p = next_prime(2^int(round(length_N/2)));
q = next_prime( round(pi.n()*p) );
N = p*q;

# qbar is q + [hidden_size_random]
hidden = 200;
diff = ZZ.random_element(0, 2^hidden-1)
qbar = q + diff; 

F.<x> = PolynomialRing(Zmod(N), implementation='NTL'); 
pol = x - qbar
dd = pol.degree()

# PLAY WITH THOSE:
beta = 0.5                             # we should have q >= N^beta
epsilon = beta / 7                     # <= beta/7
mm = ceil(beta**2 / (dd * epsilon))    # optimized
tt = floor(dd * mm * ((1/beta) - 1))   # optimized
XX = ceil(N**((beta**2/dd) - epsilon)) # we should have |diff| < X

# Coppersmith
start_time = time.time()
roots = coppersmith_howgrave_univariate(pol, N, beta, mm, tt, XX)

# output
print("\n# Solutions")
print("we want to find:", qbar - q)
print("we found:", roots)
print(("in: %s seconds " % (time.time() - start_time)))
    

```






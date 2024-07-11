## Table of Contents


- **Basic Attack Strategies** — Brute-force, frequency analysis, interpolation, downgrade & cross-protocol.
- **“Brand name” cryptographic vulnerabilities** — FREAK, CRIME, POODLE, DROWN, Logjam.
- **Advanced Attack Strategies** — Oracle (Vaudenay’s Attack, Kelsey’s Attack); meet-in-the-middle, birthday, statistical bias (differential cryptanalysis, integral cryptanalysis, etc.).
- **Side-channel attacks** and their close relatives, fault attacks.
- **Attacks on public-key cryptography** — Cube root, broadcast, related message, Coppersmith’s attack, Pohlig-Hellman algorithm, number sieve, Wiener’s attack, Bleichenbacher’s attack.


Hiding the cipher implementation details from attackers might seem to confer extra security, but once the attackers figure out these details, this extra security will be silently and irreversibly lost. That’s [Kerckhoffs’ principle](https://en.wikipedia.org/wiki/Kerckhoffs%27s_principle): “The enemy knows the system.”



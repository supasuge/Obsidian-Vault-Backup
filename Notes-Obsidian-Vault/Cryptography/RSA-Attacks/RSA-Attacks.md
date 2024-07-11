## Table of Contents


In practice ![p](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-ea925e2dc8750c7f13c22ba2d84bd7fc_l3.svg "Rendered by QuickLaTeX.com") and ![q](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-d4ad2a64838272862dab571ac7d507a6_l3.svg "Rendered by QuickLaTeX.com") must have the same bit length for a strong RSA key generation but choosing too close primes can also completely ruin the security.

In fact if ![p-q < n^{\frac{1}{4}}](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-4a63dc951aac857447fb6cf14c09efd6_l3.svg "Rendered by QuickLaTeX.com"), Fermat’s factoring algorithm can factor ![n](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-3767ff142072a7200ab99e8a6b98520b_l3.svg "Rendered by QuickLaTeX.com") efficiently.

Fermat’s factoring algorithm uses the fact that :
![n = pq = {(\frac{q-p}{2})}^2-{(\frac{p+q}{2})}^2 = x^2 - y^2](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-0e0afe659971fb63715c89498c71cb97_l3.svg "Rendered by QuickLaTeX.com")
with :

![\begin{cases}x =\frac{q-p}{2}\\y =\frac{p+q}{2}\end{cases}](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-4da2d497a37dd679e79d09aa54aa7e49_l3.svg "Rendered by QuickLaTeX.com")

You can now clearly see that ![n](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-3767ff142072a7200ab99e8a6b98520b_l3.svg "Rendered by QuickLaTeX.com") can be factorized as such:

![n = (x+y)(x-y)](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-662f436926a5ec6e077435330bdbc9a1_l3.svg "Rendered by QuickLaTeX.com")

Fermat’s algorithm to find one of the factors works as follows :

1. ![a = \sqrt{n}](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-18b9dfb82f56e82f244416248b781309_l3.svg "Rendered by QuickLaTeX.com")
2. ![b = a*a-n](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-685e24b9db48079da39ed1c388266bea_l3.svg "Rendered by QuickLaTeX.com")
3. While ![b](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-4122b558e59c5b401db63f88ca534e04_l3.svg "Rendered by QuickLaTeX.com") is not a perfect square :
    1. increment ![a](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-64642f9b4093d00310a383da880b1454_l3.svg "Rendered by QuickLaTeX.com")
    2. ![b = a*a-n](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-685e24b9db48079da39ed1c388266bea_l3.svg "Rendered by QuickLaTeX.com")
4. return ![a - \sqrt(b)](https://bitsdeep.com/wp-content/ql-cache/quicklatex.com-5d6f26b763a85af5f0957fdf17fe24f8_l3.svg "Rendered by QuickLaTeX.com")


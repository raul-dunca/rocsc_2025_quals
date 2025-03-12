<img src="https://github.com/raul-dunca/rocsc_2025_quals/blob/main/.assets/mathrix_description.png">

At first glance I noticed the `decrypt` function which was defined, but the missing piece was the value of `x`, so I knew that is what I must find. Using once again my favorite tool Google, I found a write-up: https://ctftime.org/writeup/8681 which gave me an idea: what if I compute the eigenvalues of A (λ) and of Ax (λ^x) and thus I can calculate x = log λ(λ^x) (discrete log):

```python
from sage.all import *

p = 14763417175056989171
Zp = Zmod(p)
A = Matrix(Zp, [(3934133768252467709, 9711753323648742955, …
Ax = Matrix(Zp, [(8403850723876965368, 9347688063520705231, …
Ak = Matrix(Zp, [(757411405122966805, 4459944179884813399, …

J, P = A.jordan_form(transformation=True)
Jx = P.inverse() * Ax * P

lambdas = [J[i, i] for i in range(8)]
lambdas_x = [Jx[i, i] for i in range(8)]

x_values = []
for i in range(8):
    x_i = discrete_log(Zp(lambdas_x[i]), Zp(lambdas[i]))
    x_values.append(x_i)

print(x_values)
```

This returned 3 different values for x in the list, but I used the one that appeared the most, and finally I used the given `decrypt` function to get the flag:

```python
C = [(6915558338014438117, 12451042222793413637, …
Ak = [(757411405122966805, 4459944179884813399, …
p=14763417175056989171
x=1940316715240441500
flag = decrypt(p, x, Ak, C)
print(f"CTF{{{flag}}}")
```


`CTF{5875a6dc26d5971ca01d785f9724d184cfb56f1b9be25f0f52d9b0981e600484}`

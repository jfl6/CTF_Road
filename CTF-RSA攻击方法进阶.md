![](https://cdn.nlark.com/yuque/0/2025/png/52728443/1741788300665-4e1e3729-c458-4b1a-8494-3d6c1ac83c0a.png)![](https://cdn.nlark.com/yuque/0/2025/png/52728443/1741788304675-fb1f5f49-f150-45af-b767-6512954703f5.png)

参考：

[Coppersmith 攻击](https://www.ruanx.net/coppersmith/)

[https://al3xei709.github.io/2024/01/29/Crypto-Papers-Read-Recurrent/](https://al3xei709.github.io/2024/01/29/Crypto-Papers-Read-Recurrent/)

[https://lazzzaro.github.io/2020/05/06/crypto-RSA/index.html](https://lazzzaro.github.io/2020/05/06/crypto-RSA/index.html)

在线sagemath：[Sage Cell Server](https://sagecell.sagemath.org/)

## 重要的数论知识
### 费马小定理
![](https://cdn.nlark.com/yuque/0/2025/png/52728443/1742713897984-a3f44293-33bd-458c-ad73-b6baca5032a6.png)

### 欧拉定理
![](https://cdn.nlark.com/yuque/0/2025/png/52728443/1742714055442-1891fa6c-a470-483a-b9d8-71125621b752.png)

## 手撕私钥（其实也不用）
参考：[手撕PEM密钥（RSA）](https://0xffff.one/d/1285)

[【CTF-RSA】[GHCTF 2024 新生赛]Crypto2042(手撕密钥)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1xZ42177Ar?vd_source=77cebda5641db34416ba748c55ccdda0&spm_id_from=333.788.videopod.sections)

### 1.先base64解码，再hex加密
### 2.寻找相同的字段及根据样例分类
```shell
308204a3
0201
00
0282010100 
c3a2548b12a6a086632f247541cfc3ff799f4c2dd8bea2a27a72ccecdb7ff3752aa587a0f6b3b0d36c4bb48aa005d57256423e3249d0741e6d0755bf6e8f7933c26398dc62e6b704cd72b80391e22ad3712bd842744d4622defc5dc4f514ba0cf3acf11689e40d0f0bc475cf6d4db6f84c7ca2fdbaf694bf044c19b8aa21a9b214538899525b4d616da4695b1abc0670e0dc90710090a228e9addad064b57a5e30bfdfeb97e9b68b6994be3561e6d4cd7a50775e205f41d6036e89f3b4c67b805b14443630485f0ff75bc395bd1b91529a58d966207841b7d5f81f3820e85e15956ea003e9f240f4959d90e3c3a9402ca850abd952a73916b65fd57d1dd3b997 # n

0203
010001 # e

0282010100 # d
ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff

02818100 # p
ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff

02818100 # q
ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff

028180 # dp
64505ecbfe4a150d6738beb17b4d2b7a1197c7135017fee8ca6d24cc8e81062e1ac18678142b6e1a28807e472c392d148658459f58047a69e5d9fd8417bf0c4c7379cd533915263909ff17c094ff7c4cadb967424247bb27155838b36c1097958a272c6ac5de59e48bd673d84b76b9f93e34dbb2d47638a421bbb9d3c7534fa5
028180 # dq
18fadd29bf99c9f77277efbdd965890b4821becd76414a052d702a1019305ae1072bdcc2de79418391be2a34805da96b104741a22172e4a08c5c98eafae91b3e1e5eb4303d2459003abb3e7f6893a165cf293b4bc6546dfe45b280e314f166c868a69e0255d0ab336d12a975dadce12aaf8f04735688112ff29993bc1ae68ec1
028180 # p,q
1e195af39f3f30853ac7a50d8ca3cf64ee6f5458dd8aef235768de3978da7872ff502873a1b75cb8b800883c831246c77e9a3e3a7105f7b51d389c061b544e1071abf036fc47754f0c450ef0cf8e10dcd49b3f2cbd2ac6168f67fc580a018665fb6342a3d3cbe4bada9a5855c4f18c338deaf341b2c6ad194233b868250b62df
```

### 其实用openssl只要结构没乱也可以分析出来
```shell
┌──(kali㉿kali)-[~/Desktop]
└─$ openssl rsa -in 1.pem -text -noout   

Private-Key: (2048 bit, 2 primes)
modulus:
    00:c3:a2:54:8b:12:a6:a0:86:63:2f:24:75:41:cf:
    c3:ff:79:9f:4c:2d:d8:be:a2:a2:7a:72:cc:ec:db:
    7f:f3:75:2a:a5:87:a0:f6:b3:b0:d3:6c:4b:b4:8a:
    a0:05:d5:72:56:42:3e:32:49:d0:74:1e:6d:07:55:
    bf:6e:8f:79:33:c2:63:98:dc:62:e6:b7:04:cd:72:
    b8:03:91:e2:2a:d3:71:2b:d8:42:74:4d:46:22:de:
    fc:5d:c4:f5:14:ba:0c:f3:ac:f1:16:89:e4:0d:0f:
    0b:c4:75:cf:6d:4d:b6:f8:4c:7c:a2:fd:ba:f6:94:
    bf:04:4c:19:b8:aa:21:a9:b2:14:53:88:99:52:5b:
    4d:61:6d:a4:69:5b:1a:bc:06:70:e0:dc:90:71:00:
    90:a2:28:e9:ad:da:d0:64:b5:7a:5e:30:bf:df:eb:
    97:e9:b6:8b:69:94:be:35:61:e6:d4:cd:7a:50:77:
    5e:20:5f:41:d6:03:6e:89:f3:b4:c6:7b:80:5b:14:
    44:36:30:48:5f:0f:f7:5b:c3:95:bd:1b:91:52:9a:
    58:d9:66:20:78:41:b7:d5:f8:1f:38:20:e8:5e:15:
    95:6e:a0:03:e9:f2:40:f4:95:9d:90:e3:c3:a9:40:
    2c:a8:50:ab:d9:52:a7:39:16:b6:5f:d5:7d:1d:d3:
    b9:97
publicExponent: 65537 (0x10001)
privateExponent:
    00:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff
prime1:
    00:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff
prime2:
    00:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:ff:
    ff:ff:ff:ff:ff:ff:ff:ff:ff
exponent1:
    64:50:5e:cb:fe:4a:15:0d:67:38:be:b1:7b:4d:2b:
    7a:11:97:c7:13:50:17:fe:e8:ca:6d:24:cc:8e:81:
    06:2e:1a:c1:86:78:14:2b:6e:1a:28:80:7e:47:2c:
    39:2d:14:86:58:45:9f:58:04:7a:69:e5:d9:fd:84:
    17:bf:0c:4c:73:79:cd:53:39:15:26:39:09:ff:17:
    c0:94:ff:7c:4c:ad:b9:67:42:42:47:bb:27:15:58:
    38:b3:6c:10:97:95:8a:27:2c:6a:c5:de:59:e4:8b:
    d6:73:d8:4b:76:b9:f9:3e:34:db:b2:d4:76:38:a4:
    21:bb:b9:d3:c7:53:4f:a5
exponent2:
    18:fa:dd:29:bf:99:c9:f7:72:77:ef:bd:d9:65:89:
    0b:48:21:be:cd:76:41:4a:05:2d:70:2a:10:19:30:
    5a:e1:07:2b:dc:c2:de:79:41:83:91:be:2a:34:80:
    5d:a9:6b:10:47:41:a2:21:72:e4:a0:8c:5c:98:ea:
    fa:e9:1b:3e:1e:5e:b4:30:3d:24:59:00:3a:bb:3e:
    7f:68:93:a1:65:cf:29:3b:4b:c6:54:6d:fe:45:b2:
    80:e3:14:f1:66:c8:68:a6:9e:02:55:d0:ab:33:6d:
    12:a9:75:da:dc:e1:2a:af:8f:04:73:56:88:11:2f:
    f2:99:93:bc:1a:e6:8e:c1
coefficient:
    1e:19:5a:f3:9f:3f:30:85:3a:c7:a5:0d:8c:a3:cf:
    64:ee:6f:54:58:dd:8a:ef:23:57:68:de:39:78:da:
    78:72:ff:50:28:73:a1:b7:5c:b8:b8:00:88:3c:83:
    12:46:c7:7e:9a:3e:3a:71:05:f7:b5:1d:38:9c:06:
    1b:54:4e:10:71:ab:f0:36:fc:47:75:4f:0c:45:0e:
    f0:cf:8e:10:dc:d4:9b:3f:2c:bd:2a:c6:16:8f:67:
    fc:58:0a:01:86:65:fb:63:42:a3:d3:cb:e4:ba:da:
    9a:58:55:c4:f1:8c:33:8d:ea:f3:41:b2:c6:ad:19:
    42:33:b8:68:25:0b:62:df
```

**<font style="color:#DF2A3F;">exponent1和exponent2分别对应着dp : d mod (p-1)</font>**

**<font style="color:#DF2A3F;">dq : d mod (q - 1)；</font>**

**<font style="color:#DF2A3F;">coefficient对应 ( inverse of q )mod p</font>**

## 1.CopperSmith攻击（已知高位或低位）
### 简介
Coppersmith 是干了这么一件事：今有一个 ![image](https://cdn.nlark.com/yuque/__latex/c9a277d40d3a0b3afb85cdb557908945.svg) 阶的多项式 ![image](https://cdn.nlark.com/yuque/__latex/18f3c2855f0e85a1ac2257f64d917144.svg)，那么可以：

+ 在模 ![image](https://cdn.nlark.com/yuque/__latex/df378375e7693bdcf9535661c023c02e.svg) 意义下，快速求出 ![image](https://cdn.nlark.com/yuque/__latex/93bfc3cfc442c55e0395716763a4225f.svg) 以内的根
+ 给定 ![image](https://cdn.nlark.com/yuque/__latex/6100158802e722a88c15efc101fc275b.svg)，快速求出模某个 ![image](https://cdn.nlark.com/yuque/__latex/d29c2e5f4926e5b0e9a95305650f6e54.svg) 意义下较小的根，其中 ![image](https://cdn.nlark.com/yuque/__latex/c218601001a529bfefac382ac68c6540.svg)，是 ![image](https://cdn.nlark.com/yuque/__latex/df378375e7693bdcf9535661c023c02e.svg) 的因数。

### small_roots参数问题
![](https://cdn.nlark.com/yuque/0/2025/png/52728443/1744540990052-b75903e3-e1fe-4d7e-bc02-d88b0e4ecbad.png)

### 多元coppersmith攻击
```python
# sage
def small_roots(f, bounds, m=1, d=None):
    if not d:
        d = f.degree()
    R = f.base_ring()
    N = R.cardinality()
    f /= f.coefficients().pop(0)
    f = f.change_ring(ZZ)
    G = Sequence([], f.parent())
    for i in range(m + 1):
        base = N ^ (m - i) * f ^ i
        for shifts in itertools.product(range(d), repeat=f.nvariables()):
            g = base * prod(map(power, f.variables(), shifts))
            G.append(g)
    B, monomials = G.coefficient_matrix()
    monomials = vector(monomials)
    factors = [monomial(*bounds) for monomial in monomials]
    for i, factor in enumerate(factors):
        B.rescale_col(i, factor)
    B = B.dense_matrix().LLL()
    B = B.change_ring(QQ)
    for i, factor in enumerate(factors):
        B.rescale_col(i, 1 / factor)
    H = Sequence([], f.parent().change_ring(QQ))
    for h in filter(None, B * monomials):
        H.append(h)
        I = H.ideal()
        if I.dimension() == -1:
            H.pop()
        elif I.dimension() == 0:
            roots = []
            for root in I.variety(ring=ZZ):
                root = tuple(R(root[var]) for var in f.variables())
                roots.append(root)
            return roots
    return []
```

### 已知明文高位，求低位
```python
def phase2(high_m, n, c, e):
    R.<x> = PolynomialRing(Zmod(n), implementation='NTL')
    m = high_m + x
    M = m((m^e - c).small_roots()[0])
    print(int(M))
```

### <font style="color:rgb(9, 10, 11);">已知 p 高位，求低位</font>
```python
def phase3(high_p, n, c, e):
    R.<x> = PolynomialRing(Zmod(n), implementation='NTL')
    p = high_p + x
    x0 = p.small_roots(X = 2^128, beta = 0.1)[0]
    
    P = int(p(x0))
    Q = n // P
    
    assert n == P*Q
    
    d = inverse_mod(e, (P-1)*(Q-1))
    print(hex(power_mod(c, d, n)))
```

### <font style="color:rgb(9, 10, 11);">已知 d 低位，求 p, q</font>
<font style="color:rgb(51, 51, 51);">如果知道 </font><font style="color:rgb(51, 51, 51);">d</font><font style="color:rgb(51, 51, 51);"> 的低位，低位约为 </font><font style="color:rgb(51, 51, 51);">n</font><font style="color:rgb(51, 51, 51);"> 的位数的 </font><font style="color:rgb(51, 51, 51);">1/4</font><font style="color:rgb(51, 51, 51);">就可以恢复d。</font>

<font style="color:rgb(51, 51, 51);">有些平台和不同的sagemath版本可能会报错，大数转换溢出，在macbook m2中的sagemath10.6会报错，windows平台和在线平台正常。</font>

```python
def partial_p(p0, kbits, n):
    PR.<x> = PolynomialRing(Zmod(n))
    nbits = n.nbits()
    f = 2^kbits*x + p0
    f = f.monic()
    roots = f.small_roots(X=2^(nbits//2-kbits), beta=0.4)  # find root < 2^(nbits//2-kbits) with factor >= n^0.4
    if roots:
        x0 = roots[0]
        p = gcd(2^kbits*x0 + p0, n)
        return ZZ(p)
def find_p(d0, kbits, e, n):
    X = var('X')
    for k in range(1, e+1):
        results = solve_mod([e*d0*X - k*X*(n-X+1) + k*n == X], 2^kbits)
        for x in results:
            p0 = ZZ(x[0])
            p = partial_p(p0, kbits, n)
            if p and p != 1:
                return p
if __name__ == '__main__':
    n = 
    e = 
    c = 
    d0 = 
    beta = 0.5
    nbits = n.nbits()
    kbits = d0.nbits()
    print("lower %d bits (of %d bits) is given" % (kbits, nbits))
    p = int(find_p(d0, kbits, e, n))
    print("found p: %d" % p)
    q = n//int(p)
    print("d:", inverse_mod(e, (p-1)*(q-1)))
```

### d高位攻击
```python
from tqdm import *
from Crypto.Util.number import *
def get_full_p(p_high, n,d_high,bits):
    PR.<x> = PolynomialRing(Zmod(n))    
    f = x + p_high
    f = f.monic()
    roots = f.small_roots(X=2^(bits + 4), beta=0.4)  
    if roots:
        x0 = roots[0]
        p = gcd(x0 + p_high, n)
        return ZZ(p)


def find_p_high(d_high, e, n,bits):
    PR.<X> = PolynomialRing(RealField(1000))
    for k in tqdm(range(1, e+1)):
        f=e * d_high * X - (k*n*X + k*X + X-k*X**2 - k*n)
        results = f.roots()
        if results:
            for x in results:
                p_high = int(x[0])
                p = get_full_p(p_high, n,d_high,bits)
                if p and p != 1:
                    return p


c1 = 
leak1 = 
n1 = 
e1 = 
p1 = find_p_high(leak1, e1, n1,)
q1 = n1 // p1
d1 = inverse(e1,(p1 - 1) * (p1 - 1))
m1 = pow(c1,int(d1),n1)
```

## <font style="color:rgb(9, 10, 11);">2.Boneh Durfee 攻击(已知e，d<=n^0.27)</font>
跑不出来注意参数deta，和**增大m**

### 脚本
```python
import time

"""
Setting debug to true will display more informations
about the lattice, the bounds, the vectors...
"""
debug = True

"""
Setting strict to true will stop the algorithm (and
return (-1, -1)) if we don't have a correct 
upperbound on the determinant. Note that this 
doesn't necesseraly mean that no solutions 
will be found since the theoretical upperbound is
usualy far away from actual results. That is why
you should probably use `strict = False`
"""
strict = False

"""
This is experimental, but has provided remarkable results
so far. It tries to reduce the lattice as much as it can
while keeping its efficiency. I see no reason not to use
this option, but if things don't work, you should try
disabling it
"""
helpful_only = True
dimension_min = 7 # stop removing if lattice reaches that dimension

############################################
# Functions
##########################################

# display stats on helpful vectors
def helpful_vectors(BB, modulus):
    nothelpful = 0
    for ii in range(BB.dimensions()[0]):
        if BB[ii,ii] >= modulus:
            nothelpful += 1

    print (nothelpful, "/", BB.dimensions()[0], " vectors are not helpful")

# display matrix picture with 0 and X
def matrix_overview(BB, bound):
    for ii in range(BB.dimensions()[0]):
        a = ('%02d ' % ii)
        for jj in range(BB.dimensions()[1]):
            a += '0' if BB[ii,jj] == 0 else 'X'
            if BB.dimensions()[0] < 60:
                a += ' '
        if BB[ii, ii] >= bound:
            a += '~'
        print (a)

# tries to remove unhelpful vectors
# we start at current = n-1 (last vector)
def remove_unhelpful(BB, monomials, bound, current):
    # end of our recursive function
    if current == -1 or BB.dimensions()[0] <= dimension_min:
        return BB

    # we start by checking from the end
    for ii in range(current, -1, -1):
        # if it is unhelpful:
        if BB[ii, ii] >= bound:
            affected_vectors = 0
            affected_vector_index = 0
            # let's check if it affects other vectors
            for jj in range(ii + 1, BB.dimensions()[0]):
                # if another vector is affected:
                # we increase the count
                if BB[jj, ii] != 0:
                    affected_vectors += 1
                    affected_vector_index = jj

            # level:0
            # if no other vectors end up affected
            # we remove it
            if affected_vectors == 0:
                print ("* removing unhelpful vector", ii)
                BB = BB.delete_columns([ii])
                BB = BB.delete_rows([ii])
                monomials.pop(ii)
                BB = remove_unhelpful(BB, monomials, bound, ii-1)
                return BB

            # level:1
            # if just one was affected we check
            # if it is affecting someone else
            elif affected_vectors == 1:
                affected_deeper = True
                for kk in range(affected_vector_index + 1, BB.dimensions()[0]):
                    # if it is affecting even one vector
                    # we give up on this one
                    if BB[kk, affected_vector_index] != 0:
                        affected_deeper = False
                # remove both it if no other vector was affected and
                # this helpful vector is not helpful enough
                # compared to our unhelpful one
                if affected_deeper and abs(bound - BB[affected_vector_index, affected_vector_index]) < abs(bound - BB[ii, ii]):
                    print ("* removing unhelpful vectors", ii, "and", affected_vector_index)
                    BB = BB.delete_columns([affected_vector_index, ii])
                    BB = BB.delete_rows([affected_vector_index, ii])
                    monomials.pop(affected_vector_index)
                    monomials.pop(ii)
                    BB = remove_unhelpful(BB, monomials, bound, ii-1)
                    return BB
    # nothing happened
    return BB

""" 
Returns:
* 0,0   if it fails
* -1,-1 if `strict=true`, and determinant doesn't bound
* x0,y0 the solutions of `pol`
"""
def boneh_durfee(pol, modulus, mm, tt, XX, YY):
    """
    Boneh and Durfee revisited by Herrmann and May
    
    finds a solution if:
    * d < N^delta
    * |x| < e^delta
    * |y| < e^0.5
    whenever delta < 1 - sqrt(2)/2 ~ 0.292
    """

    # substitution (Herrman and May)
    PR.<u, x, y> = PolynomialRing(ZZ)
    Q = PR.quotient(x*y + 1 - u) # u = xy + 1
    polZ = Q(pol).lift()

    UU = XX*YY + 1

    # x-shifts
    gg = []
    for kk in range(mm + 1):
        for ii in range(mm - kk + 1):
            xshift = x^ii * modulus^(mm - kk) * polZ(u, x, y)^kk
            gg.append(xshift)
    gg.sort()

    # x-shifts list of monomials
    monomials = []
    for polynomial in gg:
        for monomial in polynomial.monomials():
            if monomial not in monomials:
                monomials.append(monomial)
    monomials.sort()
    
    # y-shifts (selected by Herrman and May)
    for jj in range(1, tt + 1):
        for kk in range(floor(mm/tt) * jj, mm + 1):
            yshift = y^jj * polZ(u, x, y)^kk * modulus^(mm - kk)
            yshift = Q(yshift).lift()
            gg.append(yshift) # substitution
    
    # y-shifts list of monomials
    for jj in range(1, tt + 1):
        for kk in range(floor(mm/tt) * jj, mm + 1):
            monomials.append(u^kk * y^jj)

    # construct lattice B
    nn = len(monomials)
    BB = Matrix(ZZ, nn)
    for ii in range(nn):
        BB[ii, 0] = gg[ii](0, 0, 0)
        for jj in range(1, ii + 1):
            if monomials[jj] in gg[ii].monomials():
                BB[ii, jj] = gg[ii].monomial_coefficient(monomials[jj]) * monomials[jj](UU,XX,YY)

    # Prototype to reduce the lattice
    if helpful_only:
        # automatically remove
        BB = remove_unhelpful(BB, monomials, modulus^mm, nn-1)
        # reset dimension
        nn = BB.dimensions()[0]
        if nn == 0:
            print ("failure")
            return 0,0

    # check if vectors are helpful
    if debug:
        helpful_vectors(BB, modulus^mm)
    
    # check if determinant is correctly bounded
    det = BB.det()
    bound = modulus^(mm*nn)
    if det >= bound:
        print ("We do not have det < bound. Solutions might not be found.")
        print ("Try with highers m and t.")
        if debug:
            diff = (log(det) - log(bound)) / log(2)
            print ("size det(L) - size e^(m*n) = ", floor(diff))
        if strict:
            return -1, -1
    else:
        print ("det(L) < e^(m*n) (good! If a solution exists < N^delta, it will be found)")

    # display the lattice basis
    if debug:
        matrix_overview(BB, modulus^mm)

    # LLL
    if debug:
        print ("optimizing basis of the lattice via LLL, this can take a long time")

    BB = BB.LLL()

    if debug:
        print ("LLL is done!")

    # transform vector i & j -> polynomials 1 & 2
    if debug:
        print ("looking for independent vectors in the lattice")
    found_polynomials = False
    
    for pol1_idx in range(nn - 1):
        for pol2_idx in range(pol1_idx + 1, nn):
            # for i and j, create the two polynomials
            PR.<w,z> = PolynomialRing(ZZ)
            pol1 = pol2 = 0
            for jj in range(nn):
                pol1 += monomials[jj](w*z+1,w,z) * BB[pol1_idx, jj] / monomials[jj](UU,XX,YY)
                pol2 += monomials[jj](w*z+1,w,z) * BB[pol2_idx, jj] / monomials[jj](UU,XX,YY)

            # resultant
            PR.<q> = PolynomialRing(ZZ)
            rr = pol1.resultant(pol2)

            # are these good polynomials?
            if rr.is_zero() or rr.monomials() == [1]:
                continue
            else:
                print ("found them, using vectors", pol1_idx, "and", pol2_idx)
                found_polynomials = True
                break
        if found_polynomials:
            break

    if not found_polynomials:
        print ("no independant vectors could be found. This should very rarely happen...")
        return 0, 0
    
    rr = rr(q, q)

    # solutions
    soly = rr.roots()

    if len(soly) == 0:
        print ("Your prediction (delta) is too small")
        return 0, 0

    soly = soly[0][0]
    ss = pol1(q, soly)
    solx = ss.roots()[0][0]

    #
    return solx, soly

def example():
    ############################################
    # How To Use This Script
    ##########################################

    #
    # The problem to solve (edit the following values)
    #

    # the modulus
    N = 0xbadd260d14ea665b62e7d2e634f20a6382ac369cd44017305b69cf3a2694667ee651acded7085e0757d169b090f29f3f86fec255746674ffa8a6a3e1c9e1861003eb39f82cf74d84cc18e345f60865f998b33fc182a1a4ffa71f5ae48a1b5cb4c5f154b0997dc9b001e441815ce59c6c825f064fdca678858758dc2cebbc4d27
    # the public exponent
    e = 0x11722b54dd6f3ad9ce81da6f6ecb0acaf2cbc3885841d08b32abc0672d1a7293f9856db8f9407dc05f6f373a2d9246752a7cc7b1b6923f1827adfaeefc811e6e5989cce9f00897cfc1fc57987cce4862b5343bc8e91ddf2bd9e23aea9316a69f28f407cfe324d546a7dde13eb0bd052f694aefe8ec0f5298800277dbab4a33bb

    # the hypothesis on the private exponent (the theoretical maximum is 0.292)
    delta = 0.280 # this means that d < N^delta

    #
    # Lattice (tweak those values)
    #

    # you should tweak this (after a first run), (e.g. increment it until a solution is found)
    m = 4 # size of the lattice (bigger the better/slower)

    # you need to be a lattice master to tweak these
    t = int((1-2*delta) * m)  # optimization from Herrmann and May
    X = 2*floor(N^delta)  # this _might_ be too much
    Y = floor(N^(1/2))    # correct if p, q are ~ same size

    #
    # Don't touch anything below
    #

    # Problem put in equation
    P.<x,y> = PolynomialRing(ZZ)
    A = int((N+1)/2)
    pol = 1 + x * (A + y)

    #
    # Find the solutions!
    #

    # Checking bounds
    if debug:
        print ("=== checking values ===")
        print ("* delta:", delta)
        print ("* delta < 0.292", delta < 0.292)
        print ("* size of e:", int(log(e)/log(2)))
        print ("* size of N:", int(log(N)/log(2)))
        print ("* m:", m, ", t:", t)

    # boneh_durfee
    if debug:
        print ("=== running algorithm ===")
        start_time = time.time()

    solx, soly = boneh_durfee(pol, e, m, t, X, Y)

    # found a solution?
    if solx > 0:
        print ("=== solution found ===")
        if False:
            print ("x:", solx)
            print ("y:", soly)

        d = int(pol(solx, soly) / e)
        print ("private key found:", d)
    else:
        print ("=== no solution was found ===")

    if debug:
        print("=== %s seconds ===" % (time.time() - start_time))

if __name__ == "__main__":
    example()
```



## 3.<font style="color:rgb(9, 10, 11);">Franklin-Reiter 相关消息攻击</font>
使用同一公钥，且m1,m2具有一定线性关系

### 脚本1
```python
n = 
c1 = 
c2 = 
e = 

R.<x> = PolynomialRing(Zmod(n))
g1 = x^e - c1
g2 = (x+1)^e - c2 #根据线性关系修改

def myGcd(x, y):
    if y == 0:
        return x.monic()
    return myGcd(y, x%y)

v = myGcd(g2, g1)
M = n - v.coefficients()[0]

assert g1(M) == 0
print(hex(M))
```

## 4.<font style="color:rgb(48, 49, 51);">Schemidt-Samoa非对称密码</font>
可参考：[https://www.ruanx.net/schmidt-samoa/](https://www.ruanx.net/schmidt-samoa/)

![](https://cdn.nlark.com/yuque/0/2025/png/52728443/1744086018961-c840b236-a142-463d-9383-f36f5f7805cf.png)

```python
import gmpy2
from Crypto.Util.number import long_to_bytes

def getPQ(pub, priv):
    return gmpy2.gcd(pub, gmpy2.powmod(2, pub*priv, pub)-2)

def decrypt(pub, priv, enc):
    return gmpy2.powmod(enc, priv, getPQ(pub, priv))
```

## 5.广播攻击扩展
### 1.e不变
+ <font style="color:rgb(0, 0, 0);">本题便是广播攻击的升级版，线性填充广播攻击，在这里我们得到的是</font>

![image](https://cdn.nlark.com/yuque/__latex/f50288a7606213afc73c69d13eb9a541.svg)

<font style="color:rgb(0, 0, 0);">此时如果解决这个线性同余式呢？你能会想到这里m</font>_<font style="color:rgb(0, 0, 0);">m</font>_<font style="color:rgb(0, 0, 0);">都是这些多项式的根，是不是可以用上题的方法求多项式因子进行归约，注意这里是不行的，因为每个多项式的模不同，也就是说它们是在不同的“运算空间”中，我们不能对他们进行相互运算。</font>  
 

```python
from Crypto.Util.number import *
Cs = 
Ns = 
A = 
B = 
e = 


Fs = []
for i in range(5):
    PR.<x> = PolynomialRing(Zmod(Ns[i]))
    f = (A[i]*x + B[i])^e - Cs[i]
    f = f.monic()
    f = f.change_ring(ZZ)
    Fs.append(f)
F = crt(Fs, Ns)
M = reduce(lambda x, y: x * y, Ns)
FF = F.change_ring(Zmod(M))
m = FF.small_roots()
print(long_to_bytes(int(m[0])))
```

<font style="color:rgb(0, 0, 0);">最后进行crt（sage自带），再将得到的这个大多项式的域更换为所有模数的乘积（和广播攻击类似），这样对于这个模多项式来说，我们的根</font>![image](https://cdn.nlark.com/yuque/__latex/4760e2f007e23d820825ba241c47ce3b.svg)<font style="color:rgb(0, 0, 0);">相比模来说就显得非常小了，至少小于了</font>![image](https://cdn.nlark.com/yuque/__latex/7e5109e16965d2f55187fc34f0a8e0bc.svg)<font style="color:rgb(0, 0, 0);">，这样我们便可以利用coppersmith来进行求解根。</font>

### <font style="color:rgb(0, 0, 0);">2.e填充</font>
**<font style="color:#DF2A3F;">化为齐次方程即可</font>**

![](https://cdn.nlark.com/yuque/0/2025/png/52728443/1744124494530-101995c3-8ea1-4fc7-9f20-07872f4d1c80.png)

```python
Cs = 
Ns = 
A = 
B = 
e = [3, 3, 5, 5, 5]

P.<x>= PolynomialRing(ZZ)
Fs = []
for i in range(len(Ns)):
    f = (A[i] * x + B[i]) ** e[i] - Cs[i]
    Fs.append(f)
F = crt([Fs[0] * x^2, Fs[1] * x^2, Fs[2], Fs[3], Fs[4]], Ns)
M = prod(Ns)
FF = F.change_ring(Zmod(M))
m = FF.monic().small_roots()

from Crypto.Util.number import *
print(long_to_bytes(int(m[0])))
```

## 关于coppersmith求不到 满足![image](https://cdn.nlark.com/yuque/__latex/93bfc3cfc442c55e0395716763a4225f.svg) 以内的根
可添加偏移量，进行攻击

## 高次Rabin
普通Rabin一般是![image](https://cdn.nlark.com/yuque/__latex/295e41dd9ebdb5d97a67734b40a3f5d5.svg)（e = 2），对于高次Rabin，e一般是2的倍数，可以看出嵌套解密，<font style="color:rgb(0, 0, 0);">比如65536是2的16次方,那就需要解16次Rabin</font>

```java
from Crypto.Util.number import *

p = 
q = 
e = 8  # 可设置为任意2的幂
c = 

n = p * q

def sqrt_mod(a, p):
    assert pow(a, (p - 1) // 2, p) == 1
    if p % 4 == 3:
        return pow(a, (p + 1) // 4, p)
    # 通用求平方根可以用Tonelli-Shanks，但这里简化为递归调用
    raise NotImplementedError("当前只支持 p ≡ 3 mod 4 的素数")

def root_mod_e(c, e, p):
    m_list = [c]
    for _ in range(e.bit_length() - 1):
        next_list = []
        for m in m_list:
            try:
                r = sqrt_mod(m, p)
                next_list.extend([r, p - r])
            except:
                pass
        m_list = next_list
    return list(set(m_list))

def crt(xp, xq, p, q):
    yp = inverse(p, q)
    yq = inverse(q, p)
    return (xp * q * yq + xq * p * yp) % (p * q)

mp_list = root_mod_e(c, e, p)
mq_list = root_mod_e(c, e, q)

for mp in mp_list:
    for mq in mq_list:
        m = crt(mp, mq, p, q)
        flag = long_to_bytes(m)
        if b'NSSCTF' in flag:
            print(flag)
```


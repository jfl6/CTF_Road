可在在Jupyter中书写，table提示enter选择

## docker中启动
```python
docker run -it sagemath/sagemath:latest
```

```python
docker run -p8888:8888 sagemath/sagemath:latest sage-jupyter
```

## 基本用法
### bit长度
```python
# 调用nbits()方法获取bit长度
bit_length = n.nbits()
```

## 变量
show可以以公式形式显示

```python
x,y = var("x,y")
vars()[] ## 查看已定义函数
```

## 解方程(solve)
### 方程及方程组
方程

```python
x = var("x")
solve(x^2+x*2+2 == 0, x)
```

方程组

```python
solve([eq1 , eq2, eq3 == 1],x,y,z,soulution_dict = True)
```

### ctf中常用的
```python
solve_mod(x == y, n)
```

### find_root求一元方程
在区间上求解，只能返回一个可能解

```python
t = var("t")
find_root(cos(t) = sin(t),-3,-1)
```

### f.roots()求根
```plain
x = f.roots()
```

## 微积分
### 微分diff (求偏导)
```python
x,y = var("x,y")
f = function("f")(x,y)
show(diff(f,x,3，y,4)) # 三阶偏导
f.diff(y)
```

### 积分integral
```python
integral(x^2+1,x)
```

### 因式分解
```python
f(x) = 1/(x^2-1)
show(f.partial_fraction())
```

### 求解微分方程
desolve_laplace 

eulers_method

```python
t = var("t")
f = function("f")(t)
show(f)
de = diff(f,t) == f
desolve(de,[f,t])
```

## 多项式
#### 三种基本定义方法
```python
R = PolynomialRing(QQ,"t")
R = QQ["t"]	
R.<t> = QQ[]	# QQ为有理数环
```

#### ctf中常用的定义
```python
R.<x> = PolynomialRing(Zmod(n), implementation='NTL')
```

#### 获得生成元
```python
t = R.gen[0]
```

#### 对函数操作
```python
R.<x> = PolynomialRing(QQ)
f = x^2+4
show(f^2)
```

#### ctf中常用的small_root方法
用于求

$ f(x) \equiv 0 \mod N $

的小根

```python
m=f.small_roots(X, beta=0.5)
```

其中参数X为上界，beta一般为0～1，越小找的根越小，但可能找不到根，相当于

$ m \geq n^\beta $，但仅适用于单变量方程

#### small_root进阶处理多元问题
参考：[SageMath小白自用向导 - SeanDictionary | 折腾日记](https://seandictionary.top/sagemath/)

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


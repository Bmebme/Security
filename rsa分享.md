# RSA简单介绍

## 一些简单的数学概念

### 质数（素数）

只有1和它本身两个因数的自然数

### 欧拉函数φ(N)

欧拉函数φ(N)表示的是小于等于N的正整数并且与N互质（素）的个数。
公式：φ(x)=x(1-1/p(1))(1-1/p(2))(1-1/p(3))(1-1/p(4))…..(1-1/p(n)) 其中p(1),p(2)…p(n)为x的所有质因数。

根据素数的特点，素数的欧拉函数值就是其本身-1即φ(P）=P-1

####    欧拉函数简单的性质

![oula](https://s1.ax1x.com/2020/03/23/87A7d0.jpg)

### 欧拉定理

对于任意互素的a和n，有a^（φ(n)）≡1(mod n)
证明篇幅较长，这里就不证明了，将a和n的关系进一步缩小变为a是不能被质数p整除的正整数，套用素数的欧拉函数，就得到a^(p-1) ≡ 1 (mod p)——费马小定理

## RSA的基本条件

```

1.选取两个素数（一般很大）记为p和q
2.记n=pq，则φ(n)=(p-1)*(q-1)
3.选取任意整数记为e，使得(e,φ(n))=1，即e和φ(n)互素，如果不互素的话表明e对于mod(φ(n))没有逆元，算法不成立。
4.求e对于mod(φ(n))的逆元d，使得ed≡1 (modφ(n)),(扩展欧几里得，费马小定理或欧拉定理，特例，打表   https://blog.csdn.net/guhaiteng/article/details/52123385),也可以不用这些算法暴力尝试
5.记公钥为n和e的组合(n,e)，私钥为n和d的组合(n,d)
6.记明文为m，密文为c。
7.加密方式为c≡m^e mod n
8.解密方式为m≡c^d mod n
9.原理：将c^d mod n中的c替换m^e mod n，
        ((m^e) mod n)^d mod n
        根据蒙哥马利算法：(a * b) % p = a % p * b % p % p
        只需证明m ≡ m^(ed) mod n
        (1)(m,n)=1时:
        ed=hφ(n)+1代入m^(ed)
        m^(hφ(n)+1)=m*m^(hφ(n))
        蒙哥马利算法 ≡ m mod n * (m^(φ(n)) mod n)^h mod n
                   ≡ m mod n *1^h mod n
                   ≡ m
        (2)(m,n)≠1时:
        必有m = kp,费马小定理，有
        (kp)^(q−1)≡1(mod q)
        m^(hφ(n)+1)将φ(n)展开
        [(kp)^q−1] ^h(p−1)×kp≡kp(modq)→(kp)^(h(p−1)(q−1)+1)≡kp(modq)
        ed≡1mod(p−1)(q−1)
        得kp^(ed)≡kp mod q
        即kp^ed = kp +tq且t=rp,r是整数
        m^(ed)=m+rn 取模
        m^(ed)= m mod n
        证毕

```

##  方法1 已知pqednc求m或者已知pqednm求c

即rsa的简单应用。这里直接给出计算代码，可以直接使用
注：关于rsa的python代码大部分都需要安装gmpy2的库，安装gmpy2可能会出现很多坑，这里给出一个简单的安装教程，[点我](https://www.cnblogs.com/pcat/p/5746821.html)
 `https://www.cnblogs.com/pcat/p/5746821.html`。

```py
import gmpy2
d = 18936883706566089565183982761163360067838840777445463846869386030950038236998435836714296866300418740544099110893204385625543094093735175505576482105219020550596925375537703816973971295423941261736768673779496969415318449125906497680286964409673665181523565837769494950681114099888754750396643258941215566961
p = 0xa6055ec186de51800ddd6fcbf0192384ff42d707a55f57af4fcfb0d1dc7bd97055e8275cd4b78ec63c5d592f567c66393a061324aa2e6a8d8fc2a910cbee1ed9
q = 0xfa0f9463ea0a93b929c099320d31c277e0b0dbc65b189ed76124f5a1218f5d91fd0102a4c8de11f28be5e4d0ae91ab319f4537e97ed74bc663e972a4a9119307
e = 0x6d1fdab4ce3217b3fc32c9ed480a31d067fd57d93a9ab52b472dc393ab7852fbcb11abbebfd6aaae8032db1316dc22d3f7c3d631e24df13ef23d3b381a1c3e04abcc745d402ee3a031ac2718fae63b240837b4f657f29ca4702da9af22a3a019d68904a969ddb01bcf941df70af042f4fae5cbeb9c2151b324f387e525094c41
c = 0x7fe1a4f743675d1987d25d38111fae0f78bbea6852cba5beda47db76d119a3efe24cb04b9449f53becd43b0b46e269826a983f832abb53b7a7e24a43ad15378344ed5c20f51e268186d24c76050c1e73647523bd5f91d9b6ad3e86bbf9126588b1dee21e6997372e36c3e74284734748891829665086e0dc523ed23c386bb520
m = 0x00
n = p*q
def decrypt(c,d,n):
    m = pow(c,d,n)
    print 'm='+str(m)
def encrypt(m,e,n):
    c = pow(m,e,n)
decrypt(c,d,n)
```

## 方法二    公私钥文件相关

给出.pem公钥文件后我们应该做的事。
1.将pem文件变成我们熟悉的数字形式代码

```py
from Crypto.PublicKey import RSA

public = RSA.importKey(open("../public_rsa.pem").read())
n = public.n
e = public.e
print("n=\n%s\ne=\n%s"%(n,e))

```

2.通过一定方法拿到私钥数字，如果我们需要变成pem文件，代码：

```py
from Crypto.PublicKey import RSA

keypair = RSA.generate(1024)
keypair.p = 258631601377848992211685134376492365269
keypair.q = 286924040788547268861394901519826758027
keypair.e = 65537
keypair.n = keypair.p * keypair.q
phi_n = (keypair.p-1) * (keypair.q-1)

i = 1
while (True):
        x = (phi_n * i ) + 1
        if (x % keypair.e == 0):
            keypair.d = x / keypair.e
            break
        i += 1

private = open('private.pem','w')
private.write(keypair.exportKey())
private.close()
```

3.私钥解密文件命令。
需要安装openssl，一般kali中会集成
`openssl rsautl -decrypt -in ./RSA/flag.enc -inkey private.pem -out flag.txt`

## 方法3 已知pqe求d的代码(欧几里得算法)

```py
import gmpy2
# coding = utf-8
def computeD(fn, e):
    (x, y, r) = extendedGCD(fn, e)
    #y maybe < 0, so convert it
    if y < 0:
        return fn + y
    return y
 
def extendedGCD(a, b):
    #a*xi + b*yi = ri
    if b == 0:
        return (1, 0, a)
    #a*x1 + b*y1 = a
    x1 = 1
    y1 = 0
    #a*x2 + b*y2 = b
    x2 = 0
    y2 = 1
    while b != 0:
        q = a / b
        #ri = r(i-2) % r(i-1)
        r = a % b
        a = b
        b = r
        #xi = x(i-2) - q*x(i-1)
        x = x1 - q*x2
        x1 = x2
        x2 = x
        #yi = y(i-2) - q*y(i-1)
        y = y1 - q*y2
        y1 = y2
        y2 = y
    return(x1, y1, a)
 
p=0xa6055ec186de51800ddd6fcbf0192384ff42d707a55f57af4fcfb0d1dc7bd97055e8275cd4b78ec63c5d592f567c66393a061324aa2e6a8d8fc2a910cbee1ed9
q=0xfa0f9463ea0a93b929c099320d31c277e0b0dbc65b189ed76124f5a1218f5d91fd0102a4c8de11f28be5e4d0ae91ab319f4537e97ed74bc663e972a4a9119307
e=0x6d1fdab4ce3217b3fc32c9ed480a31d067fd57d93a9ab52b472dc393ab7852fbcb11abbebfd6aaae8032db1316dc22d3f7c3d631e24df13ef23d3b381a1c3e04abcc745d402ee3a031ac2718fae63b240837b4f657f29ca4702da9af22a3a019d68904a969ddb01bcf941df70af042f4fae5cbeb9c2151b324f387e525094c41
c=0x7fe1a4f743675d1987d25d38111fae0f78bbea6852cba5beda47db76d119a3efe24cb04b9449f53becd43b0b46e269826a983f832abb53b7a7e24a43ad15378344ed5c20f51e268186d24c76050c1e73647523bd5f91d9b6ad3e86bbf9126588b1dee21e6997372e36c3e74284734748891829665086e0dc523ed23c386bb520
 
n = p * q
fn = (p - 1) * (q - 1)
 
d = computeD(fn, e)
print 'd='+str(d)
```
#  关于n的各种分解方法
众所周知，rsa的安全性是建立在N难以被质分解，如果将N分解得到了pq，就离解题只剩下一步之遥了。
##  方法4 N过小
如果N太小了，我们就可以直接爆破得到pq，进而得到一切关于rsa的参数。比如：
```py
N = 1211809

for i in range(2, N):

    if N % i == 0:

        print i
```
##  方法5 在线网站factordb
先放网址，http://factordb.com/ ，这是一个大数质分解的数据库，如果拿到n可以直接在这里跑出来，那么就事半功倍了。
##  方法6 Wiener’s attack（低解密指数攻击）
条件：
```
    1.d < (1/3) N^(1/4)
    2.q<p<2q这个条件一般不会明着写，所以尽量看第一个
```
求e/N的连分数，使其其中的一个渐进连分数k/d满足x^2-((N-φ(n)+1))x+N=0有两个实数解，其中φ(n)=(ed-1)/k,两个实数解就分别为pq。
### 原理
∵pq在实际使用中会非常大，这就导致pq>>p+q
∴φ(n)=(p-1)(q-1)=pq-q-p+1≈pq=N
∵ed≡1 modφ(n)
即ed-1=kφ(n)
由于pq不为0,d也不为0
∴(e/φ(n))-k/d=1/(dφ(n))
∵φ(n)≈N且φ(n)非常大
∴e/N就略大于k/d
这样，借助连分数，求e/N的连分数，就可以求出k/d
举例
![xiaod1](https://img-blog.csdnimg.cn/2018122318192325.png)

某题中，给出e/N的值为17993/90581，将其写成连分数的形式如下。
![xiaod](https://img-blog.csdnimg.cn/20181223181908987.png)
将k和d的值放在

x^2-((N-φ(n)+1))x+N=0
方程中，检查是否有解，可以检查到1/5可以满足有解，此时φ(n)=89964，x1=379,x2=293
代码:
```py
def continued_fractions_expansion(numerator,denominator):#(e,N)
	result=[]
 
	divident=numerator%denominator
	quotient=numerator/denominator
	result.append(quotient)
 
	while divident!=0:
		numerator=numerator-quotient*denominator
 
		tmp=denominator
		denominator=numerator
		numerator=tmp
 
		divident=numerator%denominator
		quotient=numerator/denominator
		result.append(quotient)
 
	return result
 
def convergents(expansion):
	convergents=[(expansion[0],1)]
	for i in range(1,len(expansion)):
		numerator=1
		denominator=expansion[i]
		for j in range(i-1,-1,-1):
			numerator+=expansion[j]*denominator
			if j==0:
				break
			tmp=denominator
			denominator=numerator
			numerator=tmp
		convergents.append((numerator,denominator))#(k,d)
	return convergents
 
def newtonSqrt(n):
	approx = n/2
	better = (approx + n/approx)/2
	while better != approx:
	    approx = better
	    better = (approx + n/approx)/2
	return approx
 
def wiener_attack(cons,e,N):
	for cs in cons:
		k,d=cs
		if k==0:
			continue
		phi_N=(e*d-1)/k
		#x**2-((N-phi_N)+1)*x+N=0
		a=1
		b=-((N-phi_N)+1)
		c=N
		delta = b*b - 4*a*c
		if delta<=0:
			continue
		x1= (newtonSqrt(delta)-b)/(2*a)
		x2=-(newtonSqrt(delta)+b)/(2*a)
		if x1*x2==N:
			return [x1,x2,k,d]
 
 
N=90581
e=17993
 
expansion=continued_fractions_expansion(e,N)
cons=convergents(expansion)
 
p,q,k,d=wiener_attack(cons,e,N)
print p
print q
print k
print d
```
##  方法7 费马分解（Fermat算法）
当大整数N的两个因子p和q相近时，我们可以通过费马分解的办法很快分解大整数
### 原理
费马整数分解：对于一个任意的偶数，我们都可以通过不断提出为 2 的质因子使其最终简化为一个 2 的 n 次幂与一个奇数，因此，任意一个奇数都可以表示为：N=2*n+1
若这个奇数 N 是一个合数，根据唯一分解定理，其一定可以写成 N=c*d 的形式，不难发现，式中 c、d 均为奇数
设：c>d，令 a=(c+d)/2，b=(c-d)/2
可得：N=c*d=a*a-b*b
如果pq非常接近，可知a就约等于p或者q，b就约等于0
这样就容易分解出N并且在附近搜索到pq
代码：
```py
from gmpy2 import isqrt

def fermat(n):
    a = isqrt(n)
    b2 = a*a - n
    b = isqrt(n)
    count = 0
    while b*b != b2:
        a = a + 1
        b2 = a*a - n
        b = isqrt(b2)
        count += 1
    p = a+b
    q = a-b
    assert n == p * q
    return p, q
```
##  方法8 smallq（爆破），一般会说q<100000
由于n=pq，当q非常小时，即|p-q|较大时，我们可以直接爆破出p和q

##  方法9   Boneh Durfee Method
当d满足 d ≤ N^0.292 时，我们可以利用该方法分解N，理论上比wiener attack要强一些。
代码：
这里给出的是sage脚本，sage脚本
```py
#使用时请删除中文注释，下文中N代表模N的N，e代表公钥，delta代表N的多少次方，本代码中默认delta选择了0.18，我这里用的sage是即成py3的，如果你找到的sage是即成py2的如果报错请删除print后面的括号

import time

############################################
# Config
##########################################

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
    N = 0xc2fd2913bae61f845ac94e4ee1bb10d8531dda830d31bb221dac5f179a8f883f15046d7aa179aff848db2734b8f88cc73d09f35c445c74ee35b01a96eb7b0a6ad9cb9ccd6c02c3f8c55ecabb55501bb2c318a38cac2db69d510e152756054aaed064ac2a454e46d9b3b755b67b46906fbff8dd9aeca6755909333f5f81bf74db
    # the public exponent
    e = 0x19441f679c9609f2484eb9b2658d7138252b847b2ed8ad182be7976ed57a3e441af14897ce041f3e07916445b88181c22f510150584eee4b0f776a5a487a4472a99f2ddc95efdd2b380ab4480533808b8c92e63ace57fb42bac8315fa487d03bec86d854314bc2ec4f99b192bb98710be151599d60f224114f6b33f47e357517

    # the hypothesis on the private exponent (the theoretical maximum is 0.292)
    delta = .18 # this means that d < N^delta

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
##  方法10  yafu
yafu使用最强大的现代算法去自动化的分解输入的整数。大多数的算法都实现了多线程，让yafu能充分利用多核处理器（算法包括 SNFS, GNFS, SIQS, 和 ECM）。
#   攻击方法（一般用于看不出来怎么分解N的时候）
##  方法1   e=2(rabin算法)
rabin算法是rsa算法的衍生算法，其特点是e=2，p%4=3，q%4=3.一般先通过其他方法分解得到p，q，然后解密。
代码：
```py
def rabin_decrypt(c, p, q, e=2):
    n = p * q
    mp = pow(c, (p + 1) / 4, p)
    mq = pow(c, (q + 1) / 4, q)
    yp = gmpy2.invert(p, q)
    yq = gmpy2.invert(q, p)
    r = (yp * p * mq + yq * q * mp) % n
    rr = n - r
    s = (yp * p * mq - yq * q * mp) % n
    ss = n - s
    return (r, rr, s, ss)
```
解完会返回4个数，在这4个中试出正确答案
##  方法2   小公钥指数攻击（一般是e=3）n很大
原理：由于e很小，所以m^3会出现两种情况
```
1.m^3 < n,这样对m直接开三次方就可以直接得到c
2.m^3>n,但是同样由于e很小，导致m^3对n大不了多少，这样就可以尝试爆破
```
代码：
```py

import gmpy2
from Crypto.PublicKey import RSA

public_key = "./tmp/pubkey.pem"
cipher_file = "./tmp/flag.enc"

#读入公钥
with open(public_key, "r") as f:
    key = RSA.importKey(f)
    n = key.n
    e = key.e

#读入密文
with open(cipher_file, "r") as f:
    cipher = f.read().encode("hex")
    cipher = int(cipher, 16)
    #print(cipher)

#破解密文
def get_flag():
    i = 0
    while True:
        if(gmpy2.iroot(cipher+i*n, 3)[1] == True):
            flag_bin = int(gmpy2.iroot(cipher+x*n, 3)[0])
            flag = hex(flag_bin)[2:-1].decode("hex")
            print(flag) 
            break
        i += 1

def get_flag_for():
    for x in xrange(118600000, 118720000):
        if(gmpy2.iroot(cipher+x*n, 3)[1] == 1):
            flag_bin = int(gmpy2.iroot(cipher+x*n, 3)[0])
            flag = hex(flag_bin)[2:-1].decode("hex")
            print(flag)
            break 

if __name__ == "__main__":
    #get_flag_for()
    get_flag()
```
##  方法3   d泄漏攻击
如果我们知道一组过期的（N，e1，d1）和一组由新的e2组成的公钥及其加密的密文（N，e2，c），我们可以由（e1，d1）得到模数N的两个因子p和q，再去算e2的逆元d2，去解密密文
##  方法4   共模攻击
使用相同的模数 N 、不同的私钥，加密同一明文消息，其中gcd(e1,e2)=1
原理：
```
首先由于e1e2互质，我们根据广义欧几里得算法可以得到存在s1,s2使得
e1*s1+e2*s2 = 1
因为c1 = m^e1%n c2 = m^e2%n
所以(c1^s1*c2^s2)%n = ((m^e1%n)^s1*(m^e2%n)^s2)%n
蒙哥马利算法
(c1^s1*c2^s2)%n = ((m^e1)^s1*(m^e2)^s2)%n
即(c1^s1*c2^s2)%n = (m^(e1^s1+e2^s2))%n
又e1*s1+e2*s2 = 1
所以
(c1^s1*c2^s2)%n = (m^(1))%n 
(c1^s1*c2^s2)%n = m^%n
c1^s1*c2^s2 = m
```
所以只需要找到e1e2的广义欧几里得算法的st（在上文我用s1,s2表示）即可解出密文

代码部分:
```py
from gmpy2 import invert

def gongmogongji(n, c1, c2, e1, e2):
    def egcd(a, b):
        if b == 0:
            return a, 0
        else:
            x, y = egcd(b, a % b)
            return y, x - (a // b) * y
    s = egcd(e1, e2)
    s1 = s[0]
    s2 = s[1]

    # 求模反元素
    if s1 < 0:
        s1 = - s1
        c1 = invert(c1, n)
    elif s2 < 0:
        s2 = - s2
        c2 = invert(c2, n)
    m = pow(c1, s1, n) * pow(c2, s2, n) % n
    return m
```
##  方法5 模不互素
两个公钥的N不互素

原理：当两个N都较难分解，但是发现两个N，gcd(N1,N2)≠1时，我们可以求gcd(N1,N2)进而分解N1，N2。

代码：
```py
import gmpy2
from Crypto.Util.number import *
n1=23220619839642624127208804329329079289273497927351564011985292026254914394833691542552890810511751239656361686073628273309390314881604580204429708461587512500636158161303419916259271078173864800267063540526943181173708108324471815782985626723198144643256432774984884880698594364583949485749575467318173034467846143380574145455195152793742611717169602237969286580028662721065495380192815175057945420182742366791661416822623915523868590710387635935179876275147056396018527260488459333051132720558953142984038635223793992651637708150494964785475065404568844039983381403909341302098773533325080910057845573898984314246089
e1=65537
c1=9700614748413503291260966231863562117502096284616216707445276355274869086619796527618473213422509996843430296526594113572675840559345077344419098900818709577642324900405582499683604786981144099878021784567540654040833912063141709913653416394888766281465200682852378794478801329251224801006820925858507273130504236563822120838520746270280731121442839412258397191963036040553539697846535038841541209050503061001070909725806574230090246041891486506980939294245537252610944799573920844235221096956391095716111629998594075762507345430945523492775915790828078000453705320783486744734994213028476446922815870053311973844961

n2=22642739016943309717184794898017950186520467348317322177556419830195164079827782890660385734113396507640392461790899249329899658620250506845740531699023854206947331021605746078358967885852989786535093914459120629747240179425838485974008209140597947135295304382318570454491064938082423309363452665886141604328435366646426917928023608108470382196753292656828513681562077468846105122812084765257799070754405638149508107463233633350462138751758913036373169668828888213323429656344812014480962916088695910177763839393954730732312224100718431146133548897031060554005592930347226526561939922660855047026581292571487960929911
e2=65537
c2=20513108670823938405207629835395350087127287494963553421797351726233221750526355985253069487753150978011340115173042210284965521215128799369083065796356395285905154260709263197195828765397189267866348946188652752076472172155755940282615212228370367042435203584159326078238921502151083768908742480756781277358357734545694917591921150127540286087770229112383605858821811640935475859936319249757754722093551370392083736485637225052738864742947137890363135709796410008845576985297696922681043649916650599349320818901512835007050425460872675857974069927846620905981374869166202896905600343223640296138423898703137236463508

p1=p2=gmpy2.gcd(n1,n2)
q1=n1/p1
q2=n2/p2

d1=gmpy2.invert(e1,(p1-1)*(q1-1))
d2=gmpy2.invert(e2,(p2-1)*(q2-1))

m1=pow(c1,d1,n1)
m2=pow(c2,d2,n2)
flag=long_to_bytes(m1)+long_to_bytes(m2)
print flag
```
##  方法5   Known High Bits Factor Attack
当我们知道p的高位时，该后门算法依赖于Coppersmith partial information attack算法, sage实现该算法。
sage代码：
```py
p = 0xd7e990dec6585656512c841ac932edaf048184bac5ebf9967000000000000000
n = 0xb50193dc86a450971312d72cc8794a1d3f4977bcd1584a20c31350ac70365644074c0fb50b090f38d39beb366babd784d6555d6de3be54dad3e87a93a703abdd

kbits = 60
PR.<x> = PolynomialRing(Zmod(n))
f = x + p
x0 = f.small_roots(X=2^kbits, beta=0.4)[0]
print ("x: %s" %hex(int(x0)))
p = p+x0
print ("p: ", hex(int(p)))
assert n % p == 0
q = n/int(p)
print ("q: ", hex(int(q)))
```

##  方法6   Stereotyped messages
给了明文的高位，可以尝试使用Stereotyped messages攻击。
sage代码：b代表明文的高位，他后面有一堆0，e代表公钥，n代表模，c代表密文
```py
e = 0x3
b=0x666c6167206973203a746573743132313131313131313131313133343536000000000000000000
n = 0xf85539597ee444f3fcad07142ecf6eaae5320301244a7cedc50b2beed7e60ffa11ccf28c1a590fb81346fb16b0cecd046a1f63f0bf93185c109b8c93068ec02f
c=0xa75c3c8a19ed9c911d851917e442a8e7b425e4b7f92205ca532a2ab0f5abe6cb86d164cc61374877f9e88e7bca606b43c79f1d59deadfcc68c3db52e5fc42f0
kbits=72
PR.<x> = PolynomialRing(Zmod(n))
f = (x + b)^e-c
x0 = f.small_roots(X=2^kbits, beta=1)[0]
print （"x: %s" %hex(int(x0))）
```
##  方法7 Partial Key Exposure Attack
题目给出一组公钥n,e以及加密后的密文
给私钥d的低位
记N=pq为n比特RSA模数,e和d分别为加解密指数,ν为p和q低位相同的比特数,即p≡qmod2ν且p≠qmod2v+1.1998年,Boneh、Durfee和Frankel首先提出对RSA的部分密钥泄露攻击:当ν=1,e较小且d的低n/4比特已知时,存在关于n的多项式时间算法分解N.
sage代码：
```py
def partial_p(p0, kbits, n):
    PR.<x> = PolynomialRing(Zmod(n))
    nbits = n.nbits()

    f = 2^kbits*x + p0
    f = f.monic()
    roots = f.small_roots(X=2^(nbits//2-kbits), beta=0.3)  # find root < 2^(nbits//2-kbits) with factor >= n^0.3
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
            if p:
                return p


if __name__ == '__main__':
    n =0x56a8f8cbc72ff68e67c72718bd16d7e98150aea08780f6c4f532d20ca3c92a0fb07c959e008cbcbeac744854bc4203eb9b2996e9cf630133bc38952a2c17c27d 
    e = 0x3
    d = 0x594b6c9631c4987f588399f22466b51fc48ed449b8aae0309b5736ef0b741893
    beta = 0.5
    epsilon = beta^2/7

    nbits = n.nbits()
    kbits = 255
    d0 = d & (2^kbits-1)
    print ("lower %d bits (of %d bits) is given" % (kbits, nbits))

    p = (find_p(d0, kbits, e, n))
    print ("found p: %d" % p)
    q = n//p
    print (hex(q))
print (hex(inverse_mod(e, (p-1)*(q-1))))
```
kbits是私钥d泄露的位数255
##  方法8 Basic Broadcast Attack广播攻击
同一个加密指数e和不同且互素的模数N加密了同一个密文，并发送给了其他e个用户

原理：中国剩余定理。
![sunzidingli](https://s1.ax1x.com/2020/03/25/8jPpon.png)
代码：
```py
'''# Encoding=UTF-8
import gmpy2
# gmpy2.mpz(x)
# 初始化一个大整数x
gmpy2.mpfr(x)
# 初始化一个高精度浮点数x
C = gmpy2.powmod(M,e,n)
# 幂取模，结果是 C = (M^e) mod n
d = gmpy2.invert(e,n) # 求逆元，de = 1 mod n 
gmpy2.is_prime(n) # 判断n是不是素数
gmpy2.gcd(a,b) # 欧几里得算法
gmpy2.gcdext(a,b) # 扩展欧几里得算法
gmpy2.iroot(x,n) # x开n次根
'''
import gmpy2
import libnum
c1=gmpy2.mpz(7366067574741171461722065133242916080495505913663250330082747465383676893970411476550748394841437418105312353971095003424322679616940371123028982189502042)
n1=gmpy2.mpz(25162507052339714421839688873734596177751124036723831003300959761137811490715205742941738406548150240861779301784133652165908227917415483137585388986274803)
c2=gmpy2.mpz(21962825323300469151795920289886886562790942771546858500842179806566435767103803978885148772139305484319688249368999503784441507383476095946258011317951461)
n2=gmpy2.mpz(23976859589904419798320812097681858652325473791891232710431997202897819580634937070900625213218095330766877190212418023297341732808839488308551126409983193)
c3=gmpy2.mpz(6569689420274066957835983390583585286570087619048110141187700584193792695235405077811544355169290382357149374107076406086154103351897890793598997687053983)
n3=gmpy2.mpz(18503782836858540043974558035601654610948915505645219820150251062305120148745545906567548650191832090823482852604346478335353784501076761922605361848703623)
c4=gmpy2.mpz(4508246168044513518452493882713536390636741541551805821790338973797615971271867248584379813114125478195284692695928668946553625483179633266057122967547052)
n4=gmpy2.mpz(23383087478545512218713157932934746110721706819077423418060220083657713428503582801909807142802647367994289775015595100541168367083097506193809451365010723)
c5=gmpy2.mpz(22966105670291282335588843018244161552764486373117942865966904076191122337435542553276743938817686729554714315494818922753880198945897222422137268427611672)
n5=gmpy2.mpz(31775649089861428671057909076144152870796722528112580479442073365053916012507273433028451755436987054722496057749731758475958301164082755003195632005308493)
c6=gmpy2.mpz(17963313063405045742968136916219838352135561785389534381262979264585397896844470879023686508540355160998533122970239261072020689217153126649390825646712087)
n6=gmpy2.mpz(22246342022943432820696190444155665289928378653841172632283227888174495402248633061010615572642126584591103750338919213945646074833823905521643025879053949)
c7=gmpy2.mpz(1652417534709029450380570653973705320986117679597563873022683140800507482560482948310131540948227797045505390333146191586749269249548168247316404074014639)
n7=gmpy2.mpz(25395461142670631268156106136028325744393358436617528677967249347353524924655001151849544022201772500033280822372661344352607434738696051779095736547813043)
c8=gmpy2.mpz(15585771734488351039456631394040497759568679429510619219766191780807675361741859290490732451112648776648126779759368428205194684721516497026290981786239352)
n8=gmpy2.mpz(32056508892744184901289413287728039891303832311548608141088227876326753674154124775132776928481935378184756756785107540781632570295330486738268173167809047)
c9=gmpy2.mpz(8965123421637694050044216844523379163347478029124815032832813225050732558524239660648746284884140746788823681886010577342254841014594570067467905682359797)
n9=gmpy2.mpz(52849766269541827474228189428820648574162539595985395992261649809907435742263020551050064268890333392877173572811691599841253150460219986817964461970736553)
c0=gmpy2.mpz(13560945756543023008529388108446940847137853038437095244573035888531288577370829065666320069397898394848484847030321018915638381833935580958342719988978247)
n0=gmpy2.mpz(30415984800307578932946399987559088968355638354344823359397204419191241802721772499486615661699080998502439901585573950889047918537906687840725005496238621)
e=10
M = n1*n2*n3*n4*n5*n6*n7*n8*n9*n0
m1 = M//n1
m2 = M//n2
m3 = M//n3
m4 = M//n4
m5 = M//n5
m6 = M//n6
m7 = M//n7
m8 = M//n8
m9 = M//n9
m0 = M//n0
t1 = c1*m1*gmpy2.invert(m1,n1)
t2 = c2*m2*gmpy2.invert(m2,n2)
t3 = c3*m3*gmpy2.invert(m3,n3)
t4 = c4*m4*gmpy2.invert(m4,n4)
t5 = c5*m5*gmpy2.invert(m5,n5)
t6 = c6*m6*gmpy2.invert(m6,n6)
t7 = c7*m7*gmpy2.invert(m7,n7)
t8 = c8*m8*gmpy2.invert(m8,n8)
t9 = c9*m9*gmpy2.invert(m9,n9)
t0 = c0*m0*gmpy2.invert(m0,n0)
x = (t1+t2+t3+t4+t5+t6+t7+t8+t9+t0) % M
m=gmpy2.iroot(x,e)
print(m)
m1=854589733786598088127099154138504953368140761371523704656865879247874533963639770706597129057405#m1就是算出来的m
print(hex(m1))
m2=0x666c61677b776f305f7468335f747234696e5f69355f6c656176316e675f6733745f6f6e5f69747d
#flag=libnum.n2s(m2)
#print(flag)
```
##  方法9   padding attack
加密方法会对明文进行padding扩充之后进行加密，比如c1≡(m+padding)^e mod n
原理比较复杂，我没太读懂，https://www.anquanke.com/post/id/158944
直接放出代码：
```py
import gmpy
def getM2(a,b,c1,c2,n,e):
    a3 = pow(a,e,n)
    b3 = pow(b,e,n)
    first = c1-a3*c2+2*b3
    first = first % n
    second = e*b*(a3*c2-b3)
    second = second % n
    third = second*gmpy.invert(first,n)
    third = third % n
    fourth = (third+b)*gmpy.invert(a,n)
    return fourth % n
e=0x3
a=1
b=-1
c1=0x3aa5058306947ff46b0107b062d75cf9e497cdb1f120d02eaeca30f76492c550
c2=0x6a645739f25380a5e5b263ff5e5b4b9324381f6408a11fdaab0488209145fb3e
padding2=1
n=0xb33aebb1834845f959e05da639776d08a344abf098080dc5de04f4cbf4a1001d
m = getM2(a,b,c1,c2,n,e)-padding2
```
其中a=1
b=padding1-padding2
这边我们的padding1是第一个加密的明文与明文的差
padding2是第二个加密的明文与明文的差

##  方法10  RSA LSB Oracle Attack
特征：可以选择密文并查看其最低位
在一次RSA加密中，明文为m，模数为n，加密指数为e，密文为c。

我们可以构造出c'=((2^e)c)%n=((2^e)(m^e))%n=((2m)^e)%n， 因为m的两倍可能大于n，所以经过解密得到的明文是 m'=(2m)%n 。

我们还能够知道 m' 的最低位lsb 是1还是0。

因为n是奇数，而2m是偶数，所以如果lsb 是0，说明(2m)%n 是偶数，没有超过n，即m<n/2.0，反之则m>n/2.0 。

举个例子就能明白2%3=2 是偶数，而4%3=1 是奇数。

以此类推，构造密文c"=(4^e)c)%n 使其解密后为m"=(4m)%n ，判断m" 的奇偶性可以知道m 和 n/4 的大小关系。

所以我们就有了一个二分算法，可以在对数时间内将m的范围逼近到一个足够狭窄的空间。
代码:
```py
def brute_flag(encrypted_flag, n, e):

    flag_count = n_count = 1
    flag_lower_bound = 0
    flag_upper_bound = n
    ciphertext = encrypted_flag
    mult = 1
    while flag_upper_bound > flag_lower_bound + 1:
        sh.recvuntil("input your option:")
        sh.sendline("D")
        ciphertext = (ciphertext * pow(2, e, n)) % n
        flag_count *= 2
        n_count = n_count * 2 - 1

        print("bit = %d" % mult)
        mult += 1


        sh.recvuntil("Your encrypted message:")
        sh.sendline(str(ciphertext))

        data=sh.recvline()[:-1]
        if(data=='The plain of your decrypted message is even!'):
            flag_upper_bound = n * n_count / flag_count
        else:
            flag_lower_bound = n * n_count / flag_count
            n_count += 1
```
##  方法11  N可以分解成多个素数积
当N可以分解成多个素数的积时，这个算法就不是标准的rsa算法，但是如果该题目还套用rsa的加密逻辑的话，我们可以求解N的欧拉函数来直接跳过pq的求解。
代码：
```py
import math
from libnum import n2s,s2n


n= 703739435902178622788120837062252491867056043804038443493374414926110815100242619

#n=p1*p2*p3
p1=782758164865345954251810941
p2=810971978554706690040814093
p3=1108609086364627583447802163

n2=p1*p2*p3

#if n!=n2:
#    print '==='



e= 59159
c= 449590107303744450592771521828486744432324538211104865947743276969382998354463377



def gcd(a, b):
    while a != 0:
        a, b = b % a, a
    return b



def findModReverse(a, m):

    if gcd(a, m) != 1:
        return None
    u1, u2, u3 = 1, 0, a
    v1, v2, v3 = 0, 1, m
    while v3 != 0:
        q = u3 // v3
        v1, v2, v3, u1, u2, u3 = (u1 - q * v1), (u2 - q * v2), (u3 - q * v3), v1, v2, v3
    return u1 % m

def OLa():
    print 'ola'

    #fn=n*(1-(1.0/p1))*(1-(1.0/p2)) #.......
    #fn=n*(1-1.0/p1)*(1-1.0/p2)*(1-1.0/p3)==(p1-1)*(p2-1)*(p3-1)
    '''
    n=30
    p1=2
    p2=3
    p3=5
    print float(n),(1-1.0/p1),(1-1.0/p2),(1-1.0/p3)
    fn1=float(n)*(1-1.0/p1)*(1-1.0/p2)*(1-1.0/p3)
    print fn1
    '''
    fn=(p1-1)*(p2-1)*(p3-1)
    print fn

    return fn

def Get_d_Method2(e):
    d=findModReverse(e,OLa())
    return d


d= Get_d_Method2(e)



m=pow(c,d,n)

#print m

flag=n2s(m)
print flag

#5577446633554466577768879988
```
##  方法12  dp&dq泄漏
首先了解什么是dp和dq

dp=d%(p-1)
dq=d%(q-1)
场景：已知p，q，dp，dq，c
代码：
```py
import gmpy2
import binascii
def decrypt(dp,dq,p,q,c):
    InvQ = gmpy2.invert(q,p)
    mp = pow(c,dp,p)
    mq = pow(c,dq,q)
    m=(((mp-mq)*InvQ)%p)*q+mq
    print (binascii.unhexlify(hex(m)[2:]))

p=0xf85d730bbf09033a75379e58a8465f8048b8516f8105ce2879ce774241305b6eb4ea506b61eb7e376d4fcd425c76e80cb748ebfaf3a852b5cf3119f028cc5971
q=0xc1f34b4f826f91c5d68c5751c9af830bc770467a68699991be6e847c29c13170110ccd5e855710950abab2694b6ac730141152758acbeca0c5a51889cbe84d57
dp=0xf7b885a246a59fa1b3fe88a2971cb1ee8b19c4a7f9c1a791b9845471320220803854a967a1a03820e297c0fc1aabc2e1c40228d50228766ebebc93c97577f511
dq=0x865fe807b8595067ff93d053cc269be6a75134a34e800b741cba39744501a31cffd31cdea6078267a0bd652aeaa39a49c73d9121fafdfa7e1131a764a12fdb95
c=0xae05e0c34e2ba4ca3536987cc2cfc3f1f7f53190164d0ac50b44832f0e7224c6fdeebd2c91e3991e7d179c26b1b997295dc9724925ba431f527fba212703a0d14a34ce133661ae0b6001ee326303d6ccdc27dbd94e0987fae25a84f197c1535bdac9094bfb3846b7ca696b2e5082bea7bff804da275772ca05dd51b185a4fc30

decrypt(dp,dq,p,q,c)
```
##  方法13 dq任意之一泄漏
要求e不太大
```py
import gmpy2
import libnum
e = 65537
n = 248254007851526241177721526698901802985832766176221609612258877371620580060433101538328030305219918697643619814200930679612109885533801335348445023751670478437073055544724280684733298051599167660303645183146161497485358633681492129668802402065797789905550489547645118787266601929429724133167768465309665906113
dp = 905074498052346904643025132879518330691925174573054004621877253318682675055421970943552016695528560364834446303196939207056642927148093290374440210503657
c = 140423670976252696807533673586209400575664282100684119784203527124521188996403826597436883766041879067494280957410201958935737360380801845453829293997433414188838725751796261702622028587211560353362847191060306578510511380965162133472698713063592621028959167072781482562673683090590521214218071160287665180751

for i in range(1,65538):

    if (dp*e-1)%i == 0:

        if n%(((dp*e-1)/i)+1)==0:

            p=((dp*e-1)/i)+1

            q=n/(((dp*e-1)/i)+1)

            phi = (p-1)*(q-1)

            d = gmpy2.invert(e,phi)%phi

            print libnum.n2s(pow(c,d,n))
```
#   神器
神器1

https://github.com/D001UM3/CTF-RSA-tool

神器2

https://github.com/3summer/CTF-RSA-tool

拿到正常的参数后可以试着先在这里跑一下，这两个工具说明文档一个英文一个中文
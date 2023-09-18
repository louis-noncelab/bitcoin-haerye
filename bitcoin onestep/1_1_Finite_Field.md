# 유한체

> *비트코인의 타원곡선을 이해하려면 유한체를 알아야 합니다.
잠시 대수학의 세계에 빠져봅시다.
끈기를 갖고 천천히 따라오면 이해하기 어렵지 않습니다.*
> 

## 목차

- [군, 환, 체](#군group-환ring-체field)
- [잉여류와 잉여계](#잉여류와-잉여계)
- [유한체](#유한체finite-field)
- [구현: 유한체 원소](#구현-유한체-원소)

---


## 군(Group), 환(Ring), 체(Field)


- 현대대수학에서 수의 집합을 추상화한 것
- 이항 연산(Binary operation)
    - 집합 X가 특정 연산(⋅)에 대해 닫혀있기 위해서는 모든 X⋅X 가 X의 집합에 있어야 한다.
        
        
        |  | 자연수 | 정수 | 유리수 | 무리수 | 실수 |
        | --- | --- | --- | --- | --- | --- |
        | + | O | O | X | O | O |
        | - | X | O | X | O | O |
        | × | O | O | X | O | O |
        | ÷ | X | X | X | O | O |
- 군(群 , Group)
    - 집합 내의 임의의 원소에 대하여 특정 이항연산 ⋅ 이 정의될 때 이를 군이라 한다.
        
        $$\forall a,b,c \in G$$
        
    - 모든 원소에 대해 결합법칙이 성립해야 한다.
        
        $$(a \cdot b) \cdot c=a \cdot (b \cdot c)$$
        
    - 모든 원소의 항등원이 집합 안에 존재해야 한다.
        
        $$a \cdot e=e \cdot a=a$$
                
    - 모든 원소의 역원이 집합 안에 존재해야 한다.
        
        $$a \cdot  a^{-1} =a^{-1} \cdot a=e$$
        
        > 💡 항등원과 역원
        > - 항등원은 연산을 통해 자기 자신을 만드는 원소이다. 
        > 예를들어 덧셈 연산의 항등원은 0, 곱셈 연산의 항등원은 1 이 된다.
        > - 역원은 연산을 통해 항등원을 만드는 원소이다.
        > 예를들어 덧셈 연산의 역원은 -a, 곱셈 연산의 항등원은 1/a 가 된다.
        

        
    - 위의 법칙을 만족하면서 교환법칙이 성립하는 군을 아벨군(Abelian group) 또는 가환군(Commutative group)이라 한다.
        
        $$a \cdot b=b \cdot a$$
        
- 환(環, Ring)
    - 아벨군을 만족하는 집합 내 임의의 원소에 대해서
        
        $$\forall a,b,c \in R$$
        
    - 곱셈에 대한 결합법칙을 만족해야 한다.
        
        $$a(bc) = (ab)c$$
        
    - 분배 법칙을 만족해야 한다.
        
        $$a(b+c) = ab+ac, (a+b)c=ac+bc$$
        
    - 위의 법칙을 모두 만족하면서 곱셈에 대한 교환법칙이 성립하면 가환환(Commutative ring)이라 한다.
        
        $$ab=ba$$
        
- 체(體, Field)
    - 가환환을 만족하는 집합 내 임의의 원소에 대해서
        
        $$\forall a,b,c \in F$$
        
    - 덧셈에 대한 교환법칙을 만족해야 한다.
        
        $$a+b=b+a$$
        
    - 덧셈에 대한 결합법칙을 만족해야 한다.
        
        $$(a+b)+c=a+(b+c)$$
        
    - 0 이외의 원소에서 곱셈에 대한 역원이 존재해야 한다.
    
    > 💡 결국 집합이 체를 만족하려면 <br>
    > - 덧셈과 곱셈에 대한 교환법칙이, 결합법칙, 분배법칙 성립해야 함.<br>
    > - 덧셈과 곱셈에 대한 항등원이 존재해야 함.<br>
    > - 덧셈과 곱셈에 대한 역원이 존재해야 함. <br>
    > - → “집합 안에 사칙연산에 대한 결과가 모두 존재하면 체를 만족한다” <br>
    > - 유리수, 실수, 복소수 집합은 모두 체를 만족하나, 자연수, 정수 집합은 만족하지 못함.
    

    
---
## 잉여류와 잉여계



- “잉여(剩餘)”라는 말은 ‘남은 것’, 혹은 ‘나머지’란 뜻
- 임의의 정수를 통해 나눗셈을 정의할 때
    
    $$a=mq+r (0 \leq r < m)$$
    
- 나머지가 동일한 정수의 집합을 정의한다. 이를 잉여류라 한다.
    
    $$M_{r}$$
    
    - 예시) 임의의 정수를 3으로 나누었을 때 나머지가 1인 정수
        
        $$M_{1}=\\{1, 4, 7,  \ldots \\} (m=3)$$
        
- 모든 정수는 아래의 범주에 속하게 된다. 그리고 각 집합 간에는 공통 원소가 없다.
    
    $$M_{0},M_{1},M_{2},M_{m-1}$$
    
    - 모든 숫자는 3으로 나누었을 때 나머지가 0, 1, 2의 집합 안에 속한다.
    - 그리고 모든 정수는 나머지가 0, 1, 2 중 하나에 해당된다. (둘 이상이 나올 수 없다)
- 발생할 수 있는 모든 집합에서 각각 하나의 원소만을 뽑았을 때는 잉여계라 한다.
    - 즉 나머지가 같은 숫자가 집합안에 있으면 안된다.
    - 예시) m 이 3인 경우 가능한 잉여계는 {0, 1, 2}, {1,5}, {1,2,6}
    - m 이 3인 경우에 {0, 3}, {1, 4, 3}는 중복 원소가 있으므로 잉여계가 될 수 없다.
- 나머지가 0~m-1까지 모두 집합 안에 들어있다면 완전잉여계라 한다.
    - 예시) {0, 4, 2}, {0,1,2}
    - 시계산술에 따라 나머지가 같은 것들은 동치류로 보고 0이상 m 이하의 자연수를 원소로 하는 완전잉여계 산출이 가능하다.
        $$\\{mk,mk+1,\cdots,mk(m-1)\\} \equiv \\{0,1,\cdots,m-1\\}$$
        
        
---
## 유한체(Finite Field)


- 유한체의 기원
    - 체의 예시로는 유리수, 실수의 집합이 있으나 이는 모두 무한집합이다.
    - 유한체는 체를 만족하면서 원소의 수가 유한한 집합을 의미한다.
    - 완전잉여계는 유한한 집합이다.
    - 수학자들은 소수(p)의 완전잉여계가 체의 성질을 만족한다는 것을 찾아내었다.
> 💡 꼭 소수여야 하는 이유는 소수가 아니면 나눗셈의 역원이 존재하지 않기 때문이다. 뒤에서 알아보자.
        
- 소수의 완전잉여계는 유한체인가?
    - 완전잉여계는 원소의 수가 유한하므로 체의 조건에 만족하면 유한체라볼 수 있다.
        - 체의 조건 : 사칙 연산의 결과가 모두 집합 안에 있어야 한다.
    - 모듈로 연산(Modulo operation) : 어떤 한 숫자를 다른 숫자로 나눈 나머지를 구하는 연산으로 나머지 연산(mod)라고도 한다. 전산학에서는 ‘%’로 표현한다.
        - 예시) 7 % 3 = 1
    - 완전잉여계의 덧셈과 뺄셈
        - 완전잉여계의 덧셈과 뺄셈은 시계산술을 따른다고 생각하면 쉽다.
            - 지금 3시입니다. 47시간 후는 몇 시일까요? (3 + 47) % 12 = 11
            - 지금 3시입니다. 4시간 전은 몇 시였을까요? (3 - 4) % 12 = 11
        - p = 7인 잉여계의 예시
            - (4 + 5) % 7 = 2
            - (4 - 5) % 7 = 6
    - 완전잉여계의 곱셈
        - 곱셈은 직관적으로 수행하면 된다.
            - (4 × 3) % 7 = 5
    - 완전잉여계의 나눗셈
        - 잉여계의 나눗셈을 하려면 유클리드 호제법을 활용해야 한다.
    - 나눗셈은 역원을 곱한다는 것을 의미하므로 0을 제외한 1~6의 역원을 찾아 곱해주면 된다.
    - 예를 들어 7의 나머지로 구성된 유한체에서 3의 역원을 찾아보자.
        - 1~6의 모든 수와 7은 서로소이다. (우리는 소수의 완전잉여계를 다루고 있다.)
        - 따라서 3과 7의 최대공약수는 1이다.
        - 확장 유클리드 호제법에 넣어보자.
            - 3m+7n=1 임을 만족하는 정수 m, n을 반환해준다.
            - 7n은 0이므로 3m = 1 을 만족하는 m을 찾으면 된다.
            - m은 3의 곱셈의 역원이 된다.
            - 곱하여서 나머지가 1인 m은 5이다. (3 × 5) % 7 = 1
        - 따라서 3의 역원은 5가 된다.
    - 따라서 다음을 만족한다.
        - 5 ÷ 3 ⇒ 5 × 5 ⇒ (5 × 5) % 7 = 4
    - 소수의 완전잉여계는 체를 만족하는지 확인한다.
        - 곱셈, 덧셈에 대한 결합, 교환법칙이 성립하는가?
        - 곱셈, 덧셈에 대한 항등원과 0을 제외한 역원이 존재하는가?
    - 유한체의 나눗셈 방법
        - 나눗셈은 역원을 곱하는 것과 같다
        
        $$a\div b = a \times(1/b) = a \times{b}^{-1}$$
        
        - p 가 소수이기 때문에 페르마의 소정리에 의해 다음이 가능하다.
            
            $${b}^{p-1}=1
            \newline
            {b}^{-1}={b}^{-1}\cdot1={b}^{-1}\cdot{b}^{p-1}={b}^{p-2}
            \newline
            {b}^{-1}={b}^{p-2}$$
            
        - 따라서 다음과 같은 결론이 나온다.
            
            $$a\div b = a \times {b}^{p-2}$$
            
        - 나눗셈의 예시
            
            $$5\div 4 = (5 \times {4}^{7-2})\%7=4\times4096\%7=1$$
            
        > 💡 유클리드 호제법(-互除法, Euclidean algorithm)
        >
        > - 유클리드 호제법은 2개의 자연수의 최대 공약수를 구하는 알고리즘이다.
        > - 두 수가 서로 상대방을 나누어 원하는 수를 얻는 방식이다.
        > - 예를들어 78696과 193232의 최대공약수를 구하고 싶은 경우
        >     - 48을 18로 나누어 다음처럼 정의한다.
        >     - 그리고 아래 연산을 따라간다.
        >     - 이에 따라 최대 공약수는 6이다.               
        > $$48=18\times2+12\\18=12\times1+6\\12=6\times2+0$$
        > - 이를 일반화 하면 다음처럼 나타낼 수 있다
        > $$a = b{q_1}+r_1\\b=r_1q_2+r_2\\r_1=r_2q_3+r_3\\...\\r_{n-2}=r_{n-1}q_{n}+r_n\\r_{n-1}=r_nq_{n+1}$$
            
        <br>
    
> 💡 확장된 유클리드 호제법 (Extended -)
>
> - 두 정수 a, b의 최대 공약수가 g라고 할 때, 아래를 만족하는 m, n이 존재한다는 정의이다.
>
> $$am+bn=g$$
> 
> - 검증은 다소 복잡하니 다음 코드를 통해서 확인해보자.

```python
# 유클리드 호제법 예시 구현
def extended_gcd(a, b):
    if b == 0:
    return a, 1, 0

gcd, x1, y1 = extended_gcd(b, a % b)

x = y1
y = x1 - (a // b) * y1

return gcd, x, y

# 사용 예시
a = 48
b = 18

gcd, x, y = extended_gcd(a, b)

print(f"GCD({a}, {b}) = {gcd}")
print(f"x = {x}, y = {y}")
print(f"validate : {a*x+b*y == gcd}")

#------ 결과 ------
GCD(48, 18) = 6
x = -1, y = 3
validate : True
```

> 💡 페르마의 소정리(Fermat’s little Theorem)
> 
> - 잉여계의 동치류에 의하여 임의의 n에 대해 다음을 만족한다.
>
> $$\\{1, 2, \cdots, p-1\\} = \\{n\%p, 2n\%p, \cdots , (p-1)n\%p\\}$$
> 
> $$ex: \\{1, 2, 3,4,5,6\\} = \\{5\%7, 10\%7, \cdots , 30\%7\\} = \\{5, 3, 1, 6, 4, 2\\}$$
> 
> - 따라서 왼쪽 집합의 원소를 다 곱한것과 오른쪽 집합의 원소를 다 곱한 것은 같다.
> 
> $$(p-1)!\%p=(p-1)!\cdot{n}^{p-1}\%p$$
> 
> - 약분하여 정리하면 다음과 같다.
>     
> $$1={n}^{p-1}\%p$$
>     
> - 결론 : 유한체에서 임의의 수에 (p-1)제곱을 하면 1이 된다.
        

## 구현: 유한체 원소


```python
class FieldElement:
		#이 클래스는 유한체 안의 하나의 원소를 정의한다.
    def __init__(self, num, prime): 
		#생성자: 클래스를 선언할 때 불리는 함수이다.
		#self: 클래스 안에서 자기 자신을 부를 때 쓰는 변수이다.
		#num, prime: prime을 소수로 하는 num 나머지 원소이다.
        if num >= prime or num < 0:
				#원소는 소수를 넘을 수 없다. num은 prime으로 나누어진 나머지이기 때문이다.
            error = 'Num {} not in field range 0 to {}'.format(
                num, prime - 1)
            raise ValueError(error)
        self.num = num
        self.prime = prime

    def __repr__(self):
        return 'FieldElement_{}({})'.format(self.prime, self.num)

    def __eq__(self, other):
        if other is None:
            return False
        return self.num == other.num and self.prime == other.prime
				#두 원소가 같으려면 원소의 값과 소수가 같아야 한다.

    def __ne__(self, other):
        return not (self == other)

    def __add__(self, other):  #유한체 더하기
        if self.prime != other.prime:
            raise TypeError('Cannot add two numbers in different Fields')
        num = (self.num + other.num) % self.prime
				#유한체의 두 수를 더할 때는 두 수를 더한 뒤 소수로 나머지연산을 한다.
        return self.__class__(num, self.prime)

    def __sub__(self, other):   #유한체 빼기
        if self.prime != other.prime:
            raise TypeError('Cannot subtract two numbers in different Fields')
        num = (self.num - other.num) % self.prime
        return self.__class__(num, self.prime)

    def __mul__(self, other):   #유한체 곱하기
        if self.prime != other.prime:
            raise TypeError('Cannot multiply two numbers in different Fields')
        num = (self.num * other.num) % self.prime
        return self.__class__(num, self.prime)

    def __pow__(self, exponent):   #유한체 거듭제곱
        n = exponent % (self.prime - 1)
        num = pow(self.num, n, self.prime)
        return self.__class__(num, self.prime)

    def __truediv__(self, other):   #유한체 나누기
        if self.prime != other.prime:
            raise TypeError('Cannot divide two numbers in different Fields')
        num = (self.num * pow(other.num, self.prime - 2, self.prime)) % self.prime
        return self.__class__(num, self.prime)

    def __rmul__(self, coefficient):
        num = (self.num * coefficient) % self.prime
        return self.__class__(num=num, prime=self.prime)
```

```python
class FieldElementTest(TestCase):

    def test_ne(self):
        a = FieldElement(2, 31)
        b = FieldElement(2, 31)
        c = FieldElement(15, 31)
        self.assertEqual(a, b)
        self.assertTrue(a != c)
        self.assertFalse(a != b)

    def test_add(self):
        a = FieldElement(2, 31)
        b = FieldElement(15, 31)
        self.assertEqual(a + b, FieldElement(17, 31))
        a = FieldElement(17, 31)
        b = FieldElement(21, 31)
        self.assertEqual(a + b, FieldElement(7, 31))

    def test_sub(self):
        a = FieldElement(29, 31)
        b = FieldElement(4, 31)
        self.assertEqual(a - b, FieldElement(25, 31))
        a = FieldElement(15, 31)
        b = FieldElement(30, 31)
        self.assertEqual(a - b, FieldElement(16, 31))

    def test_mul(self):
        a = FieldElement(24, 31)
        b = FieldElement(19, 31)
        self.assertEqual(a * b, FieldElement(22, 31))

    def test_pow(self):
        a = FieldElement(17, 31)
        self.assertEqual(a**3, FieldElement(15, 31))
        a = FieldElement(5, 31)
        b = FieldElement(18, 31)
        self.assertEqual(a**5 * b, FieldElement(16, 31))

    def test_div(self):
        a = FieldElement(3, 31)
        b = FieldElement(24, 31)
        self.assertEqual(a / b, FieldElement(4, 31))
        a = FieldElement(17, 31)
        self.assertEqual(a**-3, FieldElement(29, 31))
        a = FieldElement(4, 31)
        b = FieldElement(11, 31)
        self.assertEqual(a**-4 * b, FieldElement(13, 31))
```

## 참고자료

---

- [Programming Bitcoin - Finite Field](https://github.com/noncelab/bitcoin-haerye/blob/main/mastering%20bitcoin/ch04.md)
- [https://gosamy.tistory.com/26](https://gosamy.tistory.com/26)
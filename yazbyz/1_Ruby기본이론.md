# Ruby 기본이론

## 1. 루비의 자료형
### 숫자 
### 문자열
``` ruby
'Hello World'
"Hello World"
```
### Null
``` ruby
nil
```
### 참, 거짓
``` ruby
true
false
```
### Array 배열
``` ruby
people = [ 'Alice', 4423, 3.14, nil, false ]
people[0] # 'Alice'
people[10] # 10
```
### 주석
``` ruby
# 이것은 주석
```
### Hash 해시
``` ruby
colors = { 'red' => 'ff0000', 'green' => '00ff00' } # 키에 따옴표가 있어야 한다
color = { red: 'ff0000', green: '00ff00' }
color['red'] # 'ff000'
```
~~아,, 화살표 뭐임...~~


## 2. 변수와 상수
### 변수
``` ruby shell
irb(main):001:0> x = 2
=> 2
irb(main):002:0> x + 2
=> 4
irb(main):003:0> hi = "hello"
=> "hello"
```
#### 스코프
전체 프로그램 > 클래스 > 메소드

#### 종류
- 지역변수 : 자신이 선언된 스코프에서만 참조 가능하다.
``` ruby
foo = 'foo in top level'
```

- 전역변수 : 어디서든 선언하거나 참조할 수 있다
``` ruby
$foo = 'foo in anywhere'
```
``` ruby
irb(main):001:0> bye = "GoodBye"
=> "GoodBye"
irb(main):002:0> def say_bye
irb(main):003:1> puts bye
irb(main):004:1> end
=> :say_bye
irb(main):005:0> say_bye
Traceback (most recent call last):
        3: from /usr/local/bin/irb:11:in `<main>'
        2: from (irb):5
        1: from (irb):3:in `say_bye'
NameError (undefined local variable or method `bye' for main:Object)
```
```ruby
irb(main):007:0> $byee = "SeeYouSoon"
=> "SeeYouSoon"
irb(main):012:0> def say_byee
irb(main):013:1> puts $byee
irb(main):014:1> end
=> :say_byee
irb(main):015:0> say_byee
SeeYouSoon
=> nil
```

- 인스턴스 변수 : 클래스 내에 있는 인스턴스에서 불러올 수 있다.
``` ruby
@foo = 'foo in instance'
```
~~메소드 호출하는 것도 한번에 알려주면 안되나..~~
``` ruby
class Food
    def get_food
        puts @dinner
    end
    def set_fodd
        @dinner = "steak"
    end
end
```

- 클래스 변수 : 클래스 내의 어디서든 불러올 수 있다.
``` ruby
@@foo = 'foo in instance'
```
``` ruby
class Food
    @@dinner = "steak"
    def get_food
        puts @@hello
    end
end
```

### 상수 
모든 문자를 대문자로
``` ruby
FOO_BAR = 1
 
FOO_BAR = 5 #=> ERROR 상수를 변경하려고 하면 에러를 낸다
```
``` ruby
irb(main):016:0> HELLO = 1
=> 1
irb(main):017:0> HELLO = 100
(irb):17: warning: already initialized constant HELLO
(irb):16: warning: previous definition of HELLO was here
=> 100
```
## 3. 연산자
- 대입
``` ruby
=
```
``` ruby
# = 은 오른쪽의 값을 왼쪽의 변수에 대입한다는 의미
a = 1
```

- 산술
``` ruby
+
-
*
/

%
```
``` ruby
# + 덧셈
1+2       #=> 3
'a' + "b" #=> 'ab'
# - 뺄셈
1-2       #=> -1
# * 곱셈
4*2.1     #=> 8.4
[1,'h']*2 #=> [1, "h", 1, "h"]
# ** 제곱
4**2      #=> 16
# / 나눗셈
5/2       #=> 2
5.0/2.0   #=> 2.5
# % 나머지
5%3       #=> 2
```

- 비교
``` ruby
==
!=
<
<=
>
>=
```
```ruby
1 == 2       #=> false
'a' == "a"   #=> true
1 != 2       #=> true
'a' != "a"   #=> false
1 < 2        #=> true
5.3 < 5.3    #=> false
1 <= 2       #=> true
5 <= 5       #=> true
```

- 논리
``` ruby
&& # and 
|| # or
! # not
```
``` ruby
!(1==1)                 #=> false
(1+2<=3) and 'a' != "a" #=> false
(1+2<=3) or 'a' != "a"  #=> true
a ** 3 % 3 > 5          #=> false
```

- 단항연산자

- 범위연산자

- 평행연산자

- 비트 연산자

## 4. 기본적인 제어구조
### 조건문
``` ruby
if 조건절1
    조건 1이 참일때
elsif 조건2
    조건 2가 참일때
else
    둘다 거짓일때
end
```
``` ruby
unless 조건절1
    조건 1이 거짓일때
else
    조건 1이 참일때
end
```
~~와 독특한데 이게 필요한가..?~~
``` ruby
# 짝수인지 판별
num = 2
if num%2 == 0
    puts "even"
else
    puts "odd"
end
```

### 반복문
#### while 문
``` ruby
while 조건
    조건 참일때 실행
end
```
#### for 문
``` ruby
# 범위에 대한 반복
for 변수 in 범위
    변수에 범위 값이 담겨서 사용 가능하다
end
# 배열에 대한 반복
for 변수 in 배열
    변수에 배열의 원소값이 담겨서 사용 가능하다
end
```
~~for each 같구만...~~

```ruby
for x in 0..10
    puts x
end
```
```ruby
array = ["hi", "hello", 3]
for i in array
    puts i
end
```
#### case 문
```ruby

```

#### each 문
```ruby

```

#### until 문
```ruby

```

#### break
```ruby

```

#### next( 다른 언어에서의 continue)
```ruby

```

#### redo
```ruby

```

#### retry
```ruby

```

## 5. 메소드의 정의
``` ruby
def peel_banana(banana)
  yummy_banana = banana.peel
  return yummy_banana
end
```
``` ruby
def is_odd? (num)
    if num%2 == 1
        puts "odd"
    else
        puts "even"
    end
end

실행> is_odd?(71)
odd
```

## Ruby 문제 풀기
### 문제 1. 3의 배수 30개를 출력하세요

``` ruby
for i in 1..30
	puts 3 * i
end
```

### 문제 2. 피보나치 수열을 제 20 항까지 출력해보세요
``` ruby
def fib(num)
	if num == 0
		return 0
	elsif num == 1
		return 1
	else 
		return fib(num-2) + fib(num-1)
	end
end

for i in 0..20
	puts fib(i)
end
```
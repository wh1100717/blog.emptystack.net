title: "CoffeeScript 学习笔记"
date: "2014-04-19"
excerpt: true
comments: true
tags:
    - CoffeeScript
    - 笔记
---

```coffeescript
# 赋值:
number   = 42
opposite = true

# 函数:
square = (x) -> x * x
cube   = (x) -> square(x) * x
fill = (container, liquid = "coffee") -> "Filling the #{container} with #{liquid}..."

# 数组:
list = [1, 2, 3, 4, 5]
song = ["do", "re", "mi", "fa", "so"]
singers = {Jagger: "Rock", Elvis: "Roll"}
singers_same = Jagger: "Rock", Elvis: "Roll"
singers_same_2 = 
    Jagger: "Rock"
    Elvis: "Roll"
bitlist = [
  1, 0, 1
  0, 0, 1
  1, 1, 0
]
kids =
  brother:
    name: "Max"
    age:  11
  sister:
    name: "Ida"
    age:  9
console.log kids.brother.name  #Ida

numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]
start   = numbers[0..2]         #[1，2，3]
middle  = numbers[3...-2]       #[4,5,6,7]
middle2 = numbers[3..-2]        #[4,5,6,7,8]
end     = numbers[-2..]         #[8,9]
copy    = numbers[..]           #[1,2,3,4,5,6,7,8,9]

# 对象:
math =
  root:   Math.sqrt
  square: square
  cube:   (x) -> x * square x

# 多参数输入:
race = (winner, runners...) ->
  console.log winner, runners

# 条件:
number = -42 if opposite

if happy and knowsIt
  clapsHands()
  chaChaCha()
else
  showIt()

console.log date = if friday then sue else jill

console.log "I knew it!" if elvis? # 存在性验证:

#循环
console.log val, index for val, index in some_list
console.log val for val in some_list
console.log val for val in some_list when val is/isnt some_value

yearsOld = max: 10, ida: 9, tim: 11
ages = for child, age of yearsOld
  "#{child} is #{age}"
console.log ages
#['max is 10', 'ida is 9', 'tim is 11']
console.log ages2 = "#{child} is #{age}" for child, age of yearsOld
#max ia 10
#ida is 9
#tim is 11

# While | Until
if this.studyingEconomics
  buy()  while supply > demand
  sell() until supply > demand

num = 6
lyrics = while num -= 1
  "#{num} little monkeys, jumping on the bed.
    One fell out and bumped his head."

# do用来动态执行函数
for filename in list
  do (filename) ->
    fs.readFile filename, (err, contents) ->
      compile filename, contents.toString()

# return
# 如果在函数中没有return语句，CoffeeScript会根据不同分支自动寻找最后一个变量值并返回。
grade = (student) ->
  if student.excellentWork
    "A+"
  else if student.okayStuff
    if student.triedHard then "B" else "B-"
  else
    "C"
eldest = if 24 > 21 then "Liz" else "Ike" #Liz
console.log six = (one = 1) + (two = 2) + (three = 3) #6

#运算符
CoffeeScript        JavaScript
is                  ===
isnt                !==
not                 !
and                 &&
or                  ||
true, yes, on       true
false, no, off      false
@, this             this
of                  in
in                  no JS equivalent
a ** b              Math.pow(a, b)
a // b              Math.floor(a / b)
a %% b              (a % b + b) % b
a ?= 15             if (a == null) {a = 15;}
a = b ? "c"         a = typeof b !== "undefined" && b !== null ? b : "c";
```



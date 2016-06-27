Swift中的语法
----
PS：其中一些语法确实不长用到，但是在开发中应该了解的

## typealias 重定义类型名字
## infix operator 运算符重载
## associativity 自定义运算符优先级和综合性
结合性(associativity)的值可取的值有left，right和none。左结合运算符跟其他优先级相同的左结合运算符写在一起时，会跟左边的操作数结合。同理，右结合运算符会跟右边的操作数结合。而非结合运算符不能跟其他相同优先级的运算符写在一起。结合性(associativity)的值默认为none，优先级(precedence)默认为100。

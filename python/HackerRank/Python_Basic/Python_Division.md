# Task
The provided code stub reads two integers, $a$ and $b$, from STDIN.

Add logic to print two lines. The first line should contain the result of integer division, $a // b$. The second line should contain the result of float division, $a / b$.

No rounding or formatting is necessary.

# Example
### Input
```
a = 3
b = 5
```
### Output
```
0
0.6
```
* The result of the integer division $3//5 = 0$
* The result of the float division is $3/5 = 0.6$

# Solution

```
if __name__ == '__main__':
    a = int(input())
    b = int(input())
    
    print(a//b)
    print(a/b)
```
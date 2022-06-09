# 772 Basic Calculator III

# 问题定义

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, `'+'`, `'-'`, `'*'`, `'/'` operators, and open `'('` and closing parentheses `')'`. The integer division should **truncate toward zero**.

You may assume that the given expression is always valid. All intermediate results will be in the range of `[-231, 231 - 1]`.

**Note:** You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

# 思路

- Construct number & save sign
- When current character is not digit and sign character is + or -, push sign*number into stack
- When current character is not digit and sign character is * or /,  modify the top of the stack
- Take care parenthesis using recursion

## Code

```python
class Solution:
    def calculate(self, s: str) -> int:              
        def dfs(s, index):
            num, stack, sign= 0, [], "+"
            i, n = index, len(s)
            while i < n:
                c = s[i]
                # contruct digit number
                if c.isdigit(): num = num*10 + int(c)
								# take care parathesis
                if c == "(":  num, i = dfs(s, i+1)
								# handle the pair of number and sign, either push it to 
                # the stack or modify the top of the stack
                if ( not c.isdigit() and c != " ") or i == n - 1:
                    if sign == "+":
                        stack.append(num)
                    elif sign == "-":
                        stack.append(-num)
                    elif sign == "*":
                        stack[-1] = stack[-1] * num
                    elif sign == "/":
                        stack[-1] = int(stack[-1] / float(num))
                    sign = c
                    num = 0
                if c == ")":  break
                i += 1
            return sum(stack), i  
        res, _ = dfs(s, 0)
        return res
```
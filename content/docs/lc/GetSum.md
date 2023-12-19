# Leetcode 371
## Sum of Two Integers
Problem Statement:

```Given two integers a and b, return the sum of the two integers without using the operators + and -.```

```Logic```:
We would be treating a and b in binary form for understanding bit operations.
First bit of ```sum``` calculated by ```xor(0^1, 1^0, 0^0, 1^1)```, but for the addition such as 1+1 = 10 we must have some ```carry```, which can be found by ```&``` operator between the two numbers. Also note that this approach takes count into the 3 bit addition in case of carry (1^1^1=1 and 1&1&1 = 1).
We proceed as follows 
```
procedure ADD(a, b):
    if a:
        carry = a AND b
        sum = a XOR b
        return ADD(carry << 1, sum)
    else:
        return sum
```


```Time Complexity:  O(log(max(a, b)))``` where "log" is the logarithm base 2.
This is because in each recursive call, the size of the input (represented by the maximum of 'a' and 'b') is reduced by half due to the left shift (carry << 1).


```Space Complexity: O(log(max(a,b)))```
This is because each recursive call consumes space on the call stack, and the maximum depth of the recursion is determined by the number of bits needed to represent the larger of the two numbers, 'a' or 'b'.


```cpp
class Solution { // Working c++ code
public:
    int getSum(int a, int b) {
        if (a)
            return getSum((a & b) << 1, a ^ b);
        return b;
    }
};
```


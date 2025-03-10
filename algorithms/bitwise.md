

#### bitwise operation

I feel obligated to comphrend bitwise operation more, after I solve the leetcode problem: [190. Reverse Bits](https://leetcode.com/problems/reverse-bits/description/). 




we note that $n = (b_i,b_{i-1},b_{i-2}...,{b_0})_{b} $ where $b$ is either 0 or 1.
for example, a number if $n = 2^{4}+2^{3}+2^{1}+2^0 = (1,1,0,1)_{b}$, 

For simplicity, we only use $i=3$ 4-bits for demonstrate.  

You should know 6 bitwise operators at least .


| Operator        | Description | Example |
|---------------|-------------|-------------|
| `& (AND)`    | Sets each bit to 1 if **both** corresponding bits are 1. |5 (0101) & 3 (0011) = 1 |
| `\| (OR)`     | Sets each bit to 1 if **at least one** corresponding bit is 1. |5 (0101) | 3 (0011) = 7 |
| `^ (XOR)`    | Sets each bit to 1 if **only one** corresponding bit is 1. |5 (0101) ^ 3 (0011) = 6 (0110)|
| `~ (NOT)`    | Inverts all bits (flips 0s to 1s and vice versa). |~5 (0101) = -6(0110)|
| `<< (Left Shift)` | Shifts bits to the **left**, filling with 0s (multiplies by 2). |5 (0101) << 1 = 10(1010)|
| `>> (Right Shift)` | Shifts bits to the **right**, discarding bits (divides by 2). |5 (0101) >> 1 = 2(0010)|


## Additional Notes:
- **AND (`&`)** is useful for **masking** bits.
- **OR (`|`)** is used for **setting** bits.
- **XOR (`^`)** is used for **flipping** bits or finding unique values.
- **NOT (`~`)** inverts bits (two’s complement for negative numbers).
- **Left Shift (`<<`)** multiplies by `2^k`, shifting `k` places.
- **Right Shift (`>>`)** divides by `2^k` (for positive numbers).



#### Brain Training 

Find the unique number in an array where every other number appears twice (LeetCode 136):

```python
def singleNumber(nums):
    result = 0
    for num in nums:
        result ^= num  # XOR each number
    return result
```

This statements are true:
- XOR of two same numbers is 0
    - a ^ a = 0 
- XOR of any number with 0 remains unchanged
    - 0 ^ b = 0
- XOR is commutative and associative
    - a^b^c^d = a^(b^c)^d = a^b^(c^d) = a^(b^c^d)


for example nums = [4, 1, 2, 1, 2]
(0100) ^ (0001) ^ (0010) ^ (0001) ^ (0010) = (0001) ^ (0001) ^ (0010) ^ (0010) ^ (0100) = 0 ^ 0 ^ 4 = 0


[190. Reverse Bits](https://leetcode.com/problems/reverse-bits/description/). 

```python
def reverseBits(n: int) -> int:
    # Step 1: Swap adjacent 16-bit halves
    n = (n >> 16) & 0x0000FFFF | (n << 16) &0xFFFF0000
    print(n)
    # Step 2: Swap adjacent 8-bit halves
    n = ((n  >> 8 ) & 0xFF00FF00) | ((n  << 8 ) & 0x00FF00FF )
    
    # Step 3: Swap adjacent 4-bit halves
    n = ((n  >> 4) & 0xF0F0F0F0) | ((n  << 4)& 0x0F0F0F0F)
    
    # Step 4: Swap adjacent 2-bit halves
    n = ((n  >> 2) & 0xCCCCCCCC) | ((n << 2) & 0x33333333)
    
    # Step 5: Swap adjacent 1-bit halves
    n = ((n >> 1) & 0xAAAAAAAA)| ((n  << 1) & 0x55555555)
    
    return n

```

00000010100101000001111010011100

0000001010010100 0001111010011100
0001111010011100 0000001010010100


00011110 10011100 00000010 10010100
10011100 00011110 10010100 00000010

10011100 00011110 10010100 00000010
1001 1100 0001 1110 1001 0100 0000 0010
1100 1001 1110 0001 0100 1001 0010 0000


[191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/description/)

```python
def hammingWeight(n: int) -> int:
    # Step 1: Divide into 16-bit halves and sum them
    n = (n & 0x55555555) + ((n >> 1) & 0x55555555)  # Count bits per 2-bit chunk
    n = (n & 0x33333333) + ((n >> 2) & 0x33333333)  # Count bits per 4-bit chunk
    n = (n & 0x0F0F0F0F) + ((n >> 4) & 0x0F0F0F0F)  # Count bits per 8-bit chunk
    n = (n & 0x00FF00FF) + ((n >> 8) & 0x00FF00FF)  # Count bits per 16-bit chunk
    n = (n & 0x0000FFFF) + ((n >> 16) & 0x0000FFFF)  # Count bits per 32-bit chunk
    
    return n

```
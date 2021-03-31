# Rasa Training for math proof

Getting an NLP Model to help train the human student

## The original expectation

### Problem statement:
Proof for sum of first n integers
Prove that 1 + 2 + ... + (n-1) + n = n(n+1)/2

### Solution by algebraic equation:

#### 1. 
Assume that the sum is S: 
Therefore: 1 + 2 + ... + (n-1) + n = S

#### 2. 
Sum is commutative, see https://www.khanacademy.org/math/cc-sixth-grade-math/cc-6th-factors-and-multiples/properties-of-numbers/a/properties-of-addition
Hint: it doesn't matter if I give you an apple first and then an orange or in reverse order, you always end up with an apple and an orange.

#### 3. 
Therefore order of addition doesn't matter on LHS.

#### 4. 
Therefore: n + (n-1) + ... + 2 + 1 = S
Hint: we have reversed the order.

#### 5. 
Now add both the equations in (1) and (4)

#### 6. 
1 + 2 + ... + (n-1) + n = S
Plus
n + (n-1) + ... + 2 + 1 = S

#### 7. 
Now do term by term addition on LHS
1         + 2              + ... + (n-1)        + n        = S
Plus
n         + (n-1)        + ... + 2              + 1        = S
---------------------------------------------------------------
(1+n)  +((n-1)+2)+ ...  +((n-1)+2) + (n+1)= S+S

#### 8. 
Note: anything special about the LHS?

#### 9. 
Each term by term addition reduces to (n+1)
Hint: 1 up and one down!

#### 10. 
So we can rewrite (7) as:
(1+n) + (1+n) + ... + (1+n) + (1+n) = 2S

#### 11. 
Note: can we reduce the LHS to something simpler?
n ( 1+n) = 2 S

#### 12. 
Therefore: S=n(n+1)/2
Hint: Divide both sides by 2

QED
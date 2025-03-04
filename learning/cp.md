# GCD
- It is the same as HCF.
- ```GCD(a1,a2,a3,…,an)=GCD(a1,a1+a2,a1+a2+a3,…,a1+a2+a3+…+an)```

The proof for this is, let us assume d is the GCD of a1, a2, a3,..., an. 
Now, in the RHS, since d divides a1 and a2, it will also divide a1 + a2. 
Next, since d divides a1, a2, and a3, it will also divide a1 + a2 + a3, and so on. Hence proved.

# Factorization of a number
- We only need to go till the square root of that number. This is because if i is a divisor of x, then x/i is also a divisor of x.

# Finding the longest consecutive number of 0s along with their left and right element
Assume we have a vector v of length n, which has various values:
1. First, create a vector called len, which represents the length at each index.
2. If v[i]==0, then len[i] = len[i-1]+1; This is kinda like prefix sum.
3. Now, just find the maximum element of the len vector. The index of that element will be our right range.
4. To find the left range, just subtract: i-len[i]+1;

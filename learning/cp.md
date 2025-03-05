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

# Recursion  
If our parameter is n, and we want to iterate from a particular smaller number i in ascending order until n, then the base condition would be:  
```
if(n>i) recursivefunc(n-1);
// now logic to do the operation on the first number i
```

If our parameter is n, and we want to iterate from n to a particular number i in descending order, then the base condition would be: 
```
if(n==i) return;
// now logic to do the operation on the number n
```

## Types of Methods of Solving Using Recursion
### 1. Parameterized Recursion
- In this, we pass additional parameters that **help** in computing the result.
- Useful when we need to maintain a running sum, count, or accumulated value.
- Example: sum of first ```n``` numbers
```
void sum(int n, int s) {
    if (n == 0) {
        cout << s << endl;
        return;
    }
    sum(n - 1, s + n);
}

int main() {
    sum(5, 0); // Output: 15 (5+4+3+2+1)
}

//or finding 1^3 + 2^3 + 3^3 + ...
int sum = 0;
int sumOfSeries(int n) {
    // code here
    //this is the parameterized way
    if(n==0) return sum;
    sum+= (n*n*n);
    sumOfSeries(n-1);
}
```

### 2. Functional Recursion
- This is the way of thinking where we assume that our recursive function will compute and give us the answer for n-1 terms. And then we add extra logic to find out for the nth term and add/subtract whatever to our recursivefunc(n-1).
- So, here the function returns a computed value instead of modifying an external parameter.
- This approach is more common for problems where the solution is composed of **sub-problems**.
- Example: sum of first ```n``` numbers
```
int sum(int n) {
    if (n == 0) return 0;
    return n + sum(n - 1);
}

int main() {
    cout << sum(5) << endl; // Output: 15
}
```

### 3. Head Recursion
- The recursive call is made before any computation.
- Used when we need to process data **after the recursive call**
- Useful for problems where we need to process elements in reverse order.
- Used in reversing arrays, linked lists, backtracking problems.
```
// Printing numbers from 1 to N
void print(int n) {
    if (n == 0) return;
    print(n - 1);  // Recursive call happens first
    cout << n << " ";  // Execution happens during the return phase
}

int main() {
    print(5);  // Output: 1 2 3 4 5
}

```
  
- The recursive calls go deeper first, then print while returning.

### 4. Tail Recursion
- The recursive call is the last operation in the function.
- Used when a problem can be solved without keeping track of previous function calls.
```
// Printing numbers from N to 1
void print(int n) {
    if (n == 0) return;
    cout << n << " ";  // Process first
    print(n - 1);  // Recursive call is the last step
}

int main() {
    print(5);  // Output: 5 4 3 2 1
}

```
- This approach can be converted into an iterative approach easily.

### 5. Indirect Recursion
- Multiple functions call each other in a cycle.
- When two or more functions mutually depend on each other.
- Used in parity-based problems (even-odd logic) or state-based toggling.
```
void even(int n);
void odd(int n);

void even(int n) {
    if (n == 0) {
        cout << "Even" << endl;
        return;
    }
    odd(n - 1);
}

void odd(int n) {
    if (n == 0) {
        cout << "Odd" << endl;
        return;
    }
    even(n - 1);
}

int main() {
    even(5);  // Output: Odd
    even(8);  // Output: Even
}
```


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
- Used when recursion is used for side effects (printing, updating global/state variables, etc.)
- When we don't necessarily need a return value, just a modification to state.
- When iterative conversion is easy (since parameterized recursion mimics loops)
- **When thinking of parameterized recursion, first always think of the solution using for loops. Once that is done, then you can easily convert it into parameterized recursion using the same logic you used in for loops.**
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

## Print Any One Subsequence Which Satisfies A Condition

- Structure:  
![image](https://github.com/user-attachments/assets/93937498-1e1a-43ac-b49f-d309fb5b17e4)

- Code:
![image](https://github.com/user-attachments/assets/7ab8bf04-04b3-4473-a6f2-3920fdf5a718)


## Counting Subsequences Based on a Condition
1. Using a variable passed by reference:
```
class Solution {
  public:
    int getPerfectSum(vector<int>& arr, int target, int ind, int& count){
        if(ind>=arr.size()){
            if(target==0){
                count++;
                return 0;
            } else return 0;
        }
        target-= arr[ind];
        getPerfectSum(arr, target, ind+1, count);
        target+=arr[ind];
        getPerfectSum(arr, target, ind+1, count);
        return count;
    }
  
    int perfectSum(vector<int>& arr, int target) {
        // code here
        int count = 0;
        getPerfectSum(arr, target, 0, count);
        return count;
        
    }
};
```

2. By returning 1 or 0

- Structure:  
![image](https://github.com/user-attachments/assets/36d5f3cf-2b33-4d6f-9cce-af11a0cde4ae)
For n recursion calls:  
![image](https://github.com/user-attachments/assets/9af1290c-0cee-4b11-bcc8-620fc18fb1e2)

- Code:
```
class Solution {
  public:
    int getPerfectSum(vector<int>& arr, int target, int ind){
        if(ind>=arr.size()){
            if(target==0){
                return 1;
            } else return 0;
        }
        target-= arr[ind];
        int left = getPerfectSum(arr, target, ind+1);
        target+=arr[ind];
        int right = getPerfectSum(arr, target, ind+1);
        return left + right;
    }
  
    int perfectSum(vector<int>& arr, int target) {
        // code here
        return getPerfectSum(arr, target, 0);
        
    }
};
```

# Bit Operations
## Bit Properties
Some properties of bitwise operations
```
a|b = a⊕b + a&b
a⊕(a&b) = (a|b)⊕b
b⊕(a&b) = (a|b)⊕a
(a&b)⊕(a|b) = a⊕b
```

Addition:
```
a+b = a|b + a&b
a+b = a⊕b + 2(a&b)
```

Subtraction:
```
a-b = (a⊕(a&b))-((a|b)⊕a)
a-b = ((a|b)⊕b)-((a|b)⊕a)
a-b = (a⊕(a&b))-(b⊕(a&b))
a-b = ((a|b)⊕b)-(b⊕(a&b))
```

### Number of Digits of a Number
It is given by ```floor(log2(X)) + 1```  
Proof:  
Lets say there is a number X = b0*2^0 + b1*2^1 + ... + bk*2^k.  
We can say that X >= 2^k and X<2^(k+1).  
So, 2^k <= X < 2^(k+1)  
Taking log2 on both sides, we will get our answer. 
floor(log2(X)) = k  
And the number of digits in X is k+1 (because 2^0 se le kar 2^k tak jaa raha hai). Hence floor(log2(X)) + 1.  

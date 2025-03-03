# Scalars in PyTorch
- PyTorch tensors are designed to feel and behave like Numpy Arrays.
- The advantage is that they can easily be used for operations on GPU.

```
import torch
x_pt = torch.tensor(25)
x_pt.shape
```

# Scalars in TensorFlow
Tensors are created using wrappers. Most-widely used is tf.Variable.
```
import tensorflow as tf
x_tf = tf.Variable(25, dtype=tf.int16)
x_tf.shape
x_tf + y_tf
tf_sum = tf.add(x_tf, y_tf) (same as above code)
tf_sum.numpy() # note that Numpy operations automatically convert tensors to Numpy arrays and vice versa
type(tf_sum.numpy())
```

# Vectors 
```
x = np.array([25,2,5])
```

## Vector Transposition
Transposing a regular 1-D array has no effect, since a second dimension has no meaning here. 
```
x_t = x.T 
```

However, if we create our vector using nested matrix-style brackets:
```
y = np.array([[25,2,5]])
y_t = y.T # this will turn into a column vector now
```

## Zero Vectors
Have no effect if added to another vector
```
z = np.zeros(3) # array([0,0,0])
```

## Vectors in PyTorch and TensorFlow
```
x_pt = torch.tensor([25,2,5]) # in pytorch
x_tf = tf.Variable([25,2,5]) # in tensorflow
```

## L2 Norm
Logic: 
```
from functools import reduce
sq = list(map(lambda y: y**2, x))
sumofsq = reduce(lambda p,q: p + q, sq)
ans = sumofsq**(1/2)
print(ans)

(or)

(25**2 + 2**2 + 5**2)**(1/2)
```

However, numpy comes with a built in norm method:
```
np.linalg.norm(x)
```

## L1 Norm
```
np.abs(25) + np.abs(2) + np.abs(5)
```


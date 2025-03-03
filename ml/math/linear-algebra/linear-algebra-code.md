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

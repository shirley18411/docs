﻿# Tensor

<!-- TOC -->

- [Tensor](#tensor)
    - [Overview](#overview)
    - [Tensor Structure](#tensor-structure)
    - [Tensor Attributes and Methods](#tensor-attributes-and-methods)
        - [Attributes](#attributes)
        - [Methods](#methods)

<!-- /TOC -->

<a href="https://gitee.com/mindspore/docs/blob/master/docs/programming_guide/source_en/tensor.md" target="_blank"><img src="./_static/logo_source.png"></a>

## Overview

Tensor is a basic data structure in the MindSpore network computing. For details about data types in tensors, see [dtype](https://www.mindspore.cn/doc/programming_guide/en/master/dtype.html).

Tensors of different dimensions represent different data. For example, a 0-dimensional tensor represents a scalar, a 1-dimensional tensor represents a vector, a 2-dimensional tensor represents a matrix, and a 3-dimensional tensor may represent the three channels of RGB images.

> All examples in this document can run in PyNative mode and do not support CPUs.
  
## Tensor Structure

During tensor creation, the `Tensor`, `float`, `int`, `bool`, `tuple`, `list`, and `NumPy.array` types can be transferred.

When `Tensor` is used as the initial value, dtype can be specified. If dtype is not specified, `int`, `float`, and `bool` correspond to `int32`, `float32`, and `bool_` respectively. Data types of the 1-dimensional tensors generated by `tuple` and `list` correspond to types of data stored in `tuple` and `list`.

A code example is as follows:

```python
import numpy as np
from mindspore import Tensor
from mindspore.common import dtype as mstype

x = Tensor(np.array([[1, 2], [3, 4]]), mstype.int32)
y = Tensor(1.0, mstype.int32)
z = Tensor(2, mstype.int32)
m = Tensor(True, mstype.bool_)
n = Tensor((1, 2, 3), mstype.int16)
p = Tensor([4.0, 5.0, 6.0], mstype.float64)

print(x, "\n\n", y, "\n\n", z, "\n\n", m, "\n\n", n, "\n\n", p)
```

The following information is displayed:

```text
[[1 2]
 [3 4]]

1

2

True

[1 2 3]

[4. 5. 6.]
```
  
## Tensor Attributes and Methods

### Attributes

Tensor attributes include shape and data type (dtype).

- shape: a tuple
- dtype: a data type of MindSpore

A code example is as follows:

```python
import numpy as np
from mindspore import Tensor
from mindspore.common import dtype as mstype

x = Tensor(np.array([[1, 2], [3, 4]]), mstype.int32)
x_shape = x.shape
x_dtype = x.dtype

print(x_shape, x_dtype)
```

The following information is displayed:

```text
(2, 2) Int32
```

### Methods

Tensor methods include `all`, `any`, and `asnumpy`. Currently, the `all` and `any` methods support only Ascend.

- `all(axis, keep_dims)`: performs the `and` operation on a specified dimension to reduce the dimension. `axis` indicates the reduced dimension, and `keep_dims` indicates whether to retain the reduced dimension.
- `any(axis, keep_dims)`: performs the `or` operation on a specified dimension to reduce the dimension. The parameter meaning is the same as that of `all`.
- `asnumpy()`: converts `Tensor` to an array of NumPy.

A code example is as follows:

```python
import numpy as np
from mindspore import Tensor
from mindspore.common import dtype as mstype

x = Tensor(np.array([[True, True], [False, False]]), mstype.bool_)
x_all = x.all()
x_any = x.any()
x_array = x.asnumpy()

print(x_all, "\n\n", x_any, "\n\n", x_array)
```

The following information is displayed:

```text
False

True

[[ True  True]
 [False False]]

```
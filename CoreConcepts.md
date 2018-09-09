# TensorFlow的核心概念

##### 学习TensorFlow的tensors、operation、models、layers和training等核心概念。顺带学习一些关于内存管理和严谨代码的tips。

## Tensors

Tensor是TensorFlow的核心概念：将一组数值转换成一维或多维的数组。一个Tensor实例拥有shape这个属性，用来生成用户期望的数组（即这个数组是几乘几的）。

基本的tensor构造函数是tf.tensor方法：

    // 2x3 Tensor
    
    const shape = [2, 3]; // 2 rows, 3 columns
    
    const a = tf.tensor([1.0, 2.0, 3.0, 10.0, 20.0, 30.0], shape);
    
    a.print(); // print Tensor values
    
    // Output: [[1 , 2 , 3 ],
    
    //          [10, 20, 30]]

    // The shape can also be inferred:
    
    const b = tf.tensor([[1.0, 2.0, 3.0], [10.0, 20.0, 30.0]]);
    
    b.print();
    
    // Output: [[1 , 2 , 3 ],
    
    //          [10, 20, 30]]

然而，这种简单的tensor，我们建议使用下列方法来创建以提高代码可读性：tf.scalar, tf.tensor1d, tf.tensor2d, tf.tensor3d 和 tf.tensor4d.

接下来的例子是用tf.tensor2d创建一样的例子

    const c = tf.tensor2d([[1.0, 2.0, 3.0], [10.0, 20.0, 30.0]]);
    c.print();
    // Output: [[1 , 2 , 3 ],
    //          [10, 20, 30]]

TensorFlow.js也提供了方便初始化全为0或1的方法，分别是tf.zeros和tf.ones：

    // 3x5 Tensor with all values set to 0
    const zeros = tf.zeros([3, 5]);
    // Output: [[0, 0, 0, 0, 0],
    //          [0, 0, 0, 0, 0],
    //          [0, 0, 0, 0, 0]]
    
    

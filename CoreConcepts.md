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
    
在TensorFlow.js里，tensor是不可变的；一旦创建，你不能改变它。只能执行生成的tensor的operations生成新的tensor。

## Variables

Variables是tensor初始化的值（#TODO）。不同于Tensor，Variables是可变的。你可以使用assign方法把tensor的值分配到Variables。

    const initialValues = tf.zeros([5]);
    const biases = tf.variable(initialValues); // initialize biases
    biases.print(); // output: [0, 0, 0, 0, 0]

    const updatedValues = tf.tensor1d([0, 1, 0, 1, 0]);
    biases.assign(updatedValues); // update values of biases
    biases.print(); // output: [0, 1, 0, 1, 0]
    
Variables的主要作用是在模型训练的时候，用来存储和更新tensor的值的。

## Operations（Ops）

tensor允许你存储数据，Ops允许你更改数据。TensorFlow.js提供了多种多样的适合线性代数和机器学习的Ops，可以直接用tensor实例来调用。因为tensor不可改变，Ops改变不了它们的值，但是Ops返回新的tensor。

一元Ops，square：

    const d = tf.tensor2d([[1.0, 2.0], [3.0, 4.0]]);
    const d_squared = d.square();
    d_squared.print();
    // Output: [[1, 4 ],
    //          [9, 16]]
    
二进制Ops，例如add、sub和mul：

    const e = tf.tensor2d([[1.0, 2.0], [3.0, 4.0]]);
    const f = tf.tensor2d([[5.0, 6.0], [7.0, 8.0]]);

    const e_plus_f = e.add(f);
    e_plus_f.print();
    // Output: [[6 , 8 ],
    //          [10, 12]]
    
甚至可以链式调用：

    const sq_sum = e.add(f).square();
    sq_sum.print();
    // Output: [[36 , 64 ],
    //          [100, 144]]

    // All operations are also exposed as functions in the main namespace,
    // so you could also do the following:
    const sq_sum = tf.square(tf.add(e, f));
    
## Models and Layers


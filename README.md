<!--<div align="center">
	<div class="TensorFlow">
  <img src="https://www.tensorflow.org/images/tf_logo_transp.png" style=": left; margin-left: 5px; margin-bottom: 5px;"><br><br>
   </div>
   <div class="TensorLayer">
    <img src="https://www.tensorflow.org/images/tf_logo_transp.png" style=": right; margin-left: 5px; margin-bottom: 5px;">
    </div>
</div>
-->
<a href="http://tensorlayer.readthedocs.io">
<div align="center">
	<img src="img/img_tensorlayer.png" width="30%" height="30%"/>
</div>
</a>


# TensorLayer: Deep learning and Reinforcement learning library for Researchers and Engineers. 

TensorLayer is designed to use by both Researchers and Engineers, it is a transparent library built on the top of [Google TensorFlow](https://www.tensorflow.org). It is designed to provide a higher-level API to TensorFlow in order to speed-up experimentations and developments. TensorLayer is easy to be extended and modified. In addition, we provide many examples and tutorials to help you to go through deep learning and reinforcement learning.

The documentation is not only for describing how to use TensorLayer but also a tutorial to walk through different type of Neural Networks, Deep Reinforcement Learning and Natural Language Processing etc.

However, different with other inflexible TensorFlow wrappers, TensorLayer assumes that you are somewhat familiar with Neural Networks and TensorFlow. A basic understanding of how TensorFlow works is required to be able to use TensorLayer skillfully.

🆕🆕🆕 Machine translation tutorial is released.

TensorLayer grow out of a need to combine the flexibility of TensorFlow with the availability of the right building blocks for training neural networks. Its development is guided by a number of design goals:

 - Transparency: Do not hide TensorFlow behind abstractions. Try to rely on TensorFlow’s functionality where possible, and follow TensorFlow’s conventions. Do not hide training process, all iteration, initialization can be managed by user.
 - Tensor: Neural networks perform on multidimensional data arrays which are referred to as “tensors”.
 - Tutorial: Providing mass format-consistent examples covering Dropout, DropConnect, Denoising Autoencoder, LSTM, CNN etc, speed up your development.
 - TPU: Tensor Processing Unit is a custom ASIC built specifically for machine learning and tailored for TensorFlow.
 - Distribution: Distributed Machine Learning is the default function of TensorFlow.
 - Compatibility: A network is abstracted to regularization, cost and outputs of each layer. Other wraping libraries for TensorFlow are easy to merged into TensorLayer, suitable for Researchers.
 - Simplicity: Be easy to use, extend and modify, to facilitate use in Research and Engineering.
 - High-Speed: The running speed under GPU support is the same with TensorFlow only. The simplicity do not sacrifice the performance.



Now, go through the [Overview](#Overview) to see how powerful it is !!!

-

####🇨🇳为了促进华人开发者的交流速度，我们建立了多种交流渠道，您可把微信号发送到 haodong_cs@163.com 申请加入。

####🇹🇭เราขอเรียนเชิญนักพัฒนาคนไทยทุกคนที่สนใจจะเข้าร่วมทีมพัฒนา TensorLayer ติดต่อสอบถามรายละเอียดเพิ่มเติมได้ที่ haodong_cs@163.com

####🇬🇧If you are in London, we can discuss face to face.

-

# Table of Contents
0. [Library Structure](#Library-Structure)
0. [Overview](#Overview)
0. [Easy to Modify](#Easytomodify)
0. [Installation](#Installation)
0. [Ways to Contribute](#Waystocontribute)
0. [Online Documentation](http://tensorlayer.readthedocs.io/en/latest/)
0. [Download PDF](https://media.readthedocs.org/pdf/tensorlayer/latest/tensorlayer.pdf)
0. [Download Epub](http://readthedocs.org/projects/tensorlayer/downloads/epub/latest/)
0. [Download HTML](http://readthedocs.org/projects/tensorlayer/downloads/htmlzip/latest/)


--
# Library Structure

```
<folder>
├── tensorlayer  		<--- library source code
│
├── setup.py			<--- use ‘python setup.py install’ or ‘pip install . -e‘, to install
├── docs 				<--- readthedocs folder
│   └── _build          <--- not included in the remote repo but can be generated in `docs` using `make html`
│   	 └──html
│			 └──index.html <--- homepage of the documentation
├── tutorials_*.py	 	<--- tutorials include NLP, DL, RL etc.
├── .. 
```
--
# Overview
More examples about Deep Learning, Reinforcement Learning and Nature Language Processing available on *[Read the Docs](http://tensorlayer.readthedocs.io/en/latest/)*, you can also download the docs file then read it locally.

0. [Fully Connected Network](#)
0. [Convolutional Neural Network](#)
0. [Recurrent Neural Network](#)
0. [Reinforcement Learning](#)
0. [Cost Function](#)

### *Fully Connected Network*
TensorLayer provides large amount of state-of-the-art Layers including Dropout, DropConnect, ResNet, Pre-train and so on.

**<font color="grey"> Placeholder: </font>**

All placeholder and variables can be initialized by the same way with Tensorflow's tutorial. For details please read *[tensorflow-placeholder](https://www.tensorflow.org/versions/master/api_docs/python/io_ops.html#placeholder)*, *[tensorflow-variables](https://www.tensorflow.org/versions/master/how_tos/variables/index.html)* and *[tensorflow-math](https://www.tensorflow.org/versions/r0.9/api_docs/python/math_ops.html)*.

```python
# For MNIST example, 28x28 images have 784 pixels, i.e, 784 inputs.
import tensorflow as tf
import tensorlayer as tl
x = tf.placeholder(tf.float32, shape=[None, 784], name='x')
y_ = tf.placeholder(tf.int64, shape=[None, ], name='y_')
```

**<font color="grey"> Rectifying Network with Dropout: </font>**

```python
# Define the network
network = tl.layers.InputLayer(x, name='input_layer')
network = tl.layers.DropoutLayer(network, keep=0.8, name='drop1')
network = tl.layers.DenseLayer(network, n_units=800, act = tf.nn.relu, name='relu1')
network = tl.layers.DropoutLayer(network, keep=0.5, name='drop2')
network = tl.layers.DenseLayer(network, n_units=800, act = tf.nn.relu, name='relu2')
network = tl.layers.DropoutLayer(network, keep=0.5, name='drop3')
network = tl.layers.DenseLayer(network, n_units=10, act = tl.activation.identity, name='output_layer')
# Start training
...
```
**<font color="grey"> Vanilla Sparse Autoencoder: </font>**


```python
# Define the network
network = tl.layers.InputLayer(x, name='input_layer')
network = tl.layers.DenseLayer(network, n_units=196, act = tf.nn.sigmoid, name='sigmoid1')
recon_layer1 = tl.layers.ReconLayer(network, x_recon=x, n_units=784, act = tf.nn.sigmoid, name='recon_layer1')
# Start pre-train
sess.run(tf.initialize_all_variables())
recon_layer1.pretrain(sess, x=x, X_train=X_train, X_val=X_val, denoise_name=None, n_epoch=200, batch_size=128, print_freq=10, save=True, save_name='w1pre_')
...
```
**<font color="grey"> Denoising Autoencoder: </font>**


```python
# Define the network
network = tl.layers.InputLayer(x, name='input_layer')
network = tl.layers.DropoutLayer(network, keep=0.5, name='denoising1')   
network = tl.layers.DenseLayer(network, n_units=196, act = tf.nn.relu, name='relu1')
recon_layer1 = tl.layers.ReconLayer(network, x_recon=x, n_units=784, act = tf.nn.softplus, name='recon_layer1')
# Start pre-train
sess.run(tf.initialize_all_variables())
recon_layer1.pretrain(sess, x=x, X_train=X_train, X_val=X_val, denoise_name='denoising1', n_epoch=200, batch_size=128, print_freq=10, save=True, save_name='w1pre_')
...
```

**<font color="grey"> Stacked Denoising Autoencoders: </font>**

```python
# Define the network
network = tl.layers.InputLayer(x, name='input_layer')
# denoise layer for Autoencoders
network = tl.layers.DropoutLayer(network, keep=0.5, name='denoising1')
# 1st layer
network = tl.layers.DropoutLayer(network, keep=0.8, name='drop1')
network = tl.layers.DenseLayer(network, n_units=800, act = tf.nn.relu, name='relu1')
x_recon1 = network.outputs
recon_layer1 = tl.layers.ReconLayer(network, x_recon=x, n_units=784, act = tf.nn.softplus, name='recon_layer1')
# 2nd layer
network = tl.layers.DropoutLayer(network, keep=0.5, name='drop2')
network = tl.layers.DenseLayer(network, n_units=800, act = tf.nn.relu, name='relu2')
recon_layer2 = tl.layers.ReconLayer(network, x_recon=x_recon1, n_units=800, act = tf.nn.softplus, name='recon_layer2')
# 3rd layer
network = tl.layers.DropoutLayer(network, keep=0.5, name='drop3')
network = tl.layers.DenseLayer(network, n_units=10, act = tl.activation.identity, name='output_layer')

sess.run(tf.initialize_all_variables())

# Print all parameters before pre-train
network.print_params()

# Pre-train Layer 1
recon_layer1.pretrain(sess, x=x, X_train=X_train, X_val=X_val, denoise_name='denoising1', n_epoch=100, batch_size=128, print_freq=10, save=True, save_name='w1pre_')
# Pre-train Layer 2
recon_layer2.pretrain(sess, x=x, X_train=X_train, X_val=X_val, denoise_name='denoising1', n_epoch=100, batch_size=128, print_freq=10, save=False)
# Start training
...
```

### *Convolutional Neural Network*

Instead of feeding the images as 1D vectors, the images can be imported as 4D matrix, where [None, 28, 28, 1] represents [batchsize, height, width, channels]. Set 'batchsize' to 'None' means data with different batchsize can all filled into the placeholder.

```python
x = tf.placeholder(tf.float32, shape=[None, 28, 28, 1])
y_ = tf.placeholder(tf.int64, shape=[None,])
```

**<font color="grey"> CNNs + MLP: </font>**

A 2 layers CNN followed by 2 fully connected layers can be defined by the following codes:

```python
network = tl.layers.InputLayer(x, name='input_layer')
network = tl.layers.Conv2dLayer(network,
                        act = tf.nn.relu,
                        shape = [5, 5, 1, 32],  # 32 features for each 5x5 patch
                        strides=[1, 1, 1, 1],
                        padding='SAME',
                        name ='cnn_layer1')     # output: (?, 28, 28, 32)
network = tl.layers.Pool2dLayer(network,
                        ksize=[1, 2, 2, 1],
                        strides=[1, 2, 2, 1],
                        padding='SAME',
                        pool = tf.nn.max_pool,
                        name ='pool_layer1',)   # output: (?, 14, 14, 32)
network = tl.layers.Conv2dLayer(network,
                        act = tf.nn.relu,
                        shape = [5, 5, 32, 64], # 64 features for each 5x5 patch
                        strides=[1, 1, 1, 1],
                        padding='SAME',
                        name ='cnn_layer2')     # output: (?, 14, 14, 64)
network = tl.layers.Pool2dLayer(network,
                        ksize=[1, 2, 2, 1],
                        strides=[1, 2, 2, 1],
                        padding='SAME',
                        pool = tf.nn.max_pool,
                        name ='pool_layer2',)   # output: (?, 7, 7, 64)
network = tl.layers.FlattenLayer(network, name='flatten_layer')                                # output: (?, 3136)
network = tl.layers.DropoutLayer(network, keep=0.5, name='drop1')                              # output: (?, 3136)
network = tl.layers.DenseLayer(network, n_units=256, act = tf.nn.relu, name='relu1')           # output: (?, 256)
network = tl.layers.DropoutLayer(network, keep=0.5, name='drop2')                              # output: (?, 256)
network = tl.layers.DenseLayer(network, n_units=10, act = tl.activation.identity, name='output_layer')    # output: (?, 10)
```
For more powerful functions, please go to *[Read the Docs](http://tensorlayer.readthedocs.io/en/latest/)*.


### *Recurrent Neural Network*

**<font color="grey"> LSTM: </font>** 

Please go to *[Understand LSTM](http://tensorlayer.readthedocs.io/en/latest/user/tutorial.html#run-the-ptb-example)*.


### *Reinforcement Learning*
To understand Reinforcement Learning, a Blog (*[Deep Reinforcement Learning: Pong from Pixels](http://karpathy.github.io/2016/05/31/rl/)*) and a Paper (*[Playing Atari with Deep Reinforcement Learning](https://www.cs.toronto.edu/~vmnih/docs/dqn.pdf)*) are recommended. To play with RL, use *[OpenAI Gym](https://github.com/openai/gym)* as benchmark is recommended.

**<font color="grey"> Pong Game: </font>**

Atari Pong Game is a single agent example. *[Pong from Pixels](http://karpathy.github.io/2016/05/31/rl/)* using 130 lines of Python only *[(Code link)](https://gist.github.com/karpathy/a4166c7fe253700972fcbc77e4ea32c5)* can be reimplemented as follow.

```python
# Policy network
network = tl.layers.InputLayer(x, name='input_layer')
network = tl.layers.DenseLayer(network, n_units= H , act = tf.nn.relu, name='relu_layer')
network = tl.layers.DenseLayer(network, n_units= 1 , act = tf.nn.sigmoid, name='output_layer')
```

For RL part, please read *[Policy Gradient](http://tensorlayer.readthedocs.io/en/latest/user/tutorial.html#understand-reinforcement-learning)*.



### *Cost Function*

TensorLayer provides a simple way to creat you own cost function. Take a MLP below for example.

```python
network = tl.InputLayer(x, name='input_layer')
network = tl.DropoutLayer(network, keep=0.8, name='drop1')
network = tl.DenseLayer(network, n_units=800, act = tf.nn.relu, name='relu1')
network = tl.DropoutLayer(network, keep=0.5, name='drop2')
network = tl.DenseLayer(network, n_units=800, act = tf.nn.relu, name='relu2')
network = tl.DropoutLayer(network, keep=0.5, name='drop3')
network = tl.DenseLayer(network, n_units=10, act = tl.activation.identity, name='output_layer')
```

**<font color="grey"> Regularization of Weights: </font>**

After initializing the variables, the informations of network parameters can be observed by using **<font color="grey">network.print_params()</font>**.

```python
sess.run(tf.initialize_all_variables())
network.print_params()
>> param 0: (784, 800) (mean: -0.000000, median: 0.000004 std: 0.035524)
>> param 1: (800,) (mean: 0.000000, median: 0.000000 std: 0.000000)
>> param 2: (800, 800) (mean: 0.000029, median: 0.000031 std: 0.035378)
>> param 3: (800,) (mean: 0.000000, median: 0.000000 std: 0.000000)
>> param 4: (800, 10) (mean: 0.000673, median: 0.000763 std: 0.049373)
>> param 5: (10,) (mean: 0.000000, median: 0.000000 std: 0.000000)
>> num of params: 1276810
```

The output of network is **<font color="grey">network.outputs</font>**, then the cross entropy can be defined as follow. Besides, to regularize the weights, the **<font color="grey">network.all_params</font>** contains all parameters of the network. In this case, **<font color="grey">network.all_params</font>** = [W1, b1, W2, b2, Wout, bout] according to param 0, 1 ... 5 shown by **<font color="grey">network.print_params()</font>**. Then max-norm regularization on W1 and W2 can be performed as follow.

```python
y = network.outputs
cross_entropy = tf.reduce_mean(tf.nn.sparse_softmax_cross_entropy_with_logits(y, y_))
cost = cross_entropy
cost = cost + tl.cost.maxnorm_regularizer(1.0)(network.all_params[0]) + tl.cost.maxnorm_regularizer(1.0)(network.all_params[2])
```
In addition, all TensorFlow's regularizers like **<font color="grey">tf.contrib.layers.l2_regularizer</font>** can be used with TensorLayer.

**<font color="grey"> Regularization of Activation Outputs: </font>**

Instance method **<font color="grey">network.print_layers()</font>** prints all outputs of different layers in order. To achieve regularization on activation output, you can use **<font color="grey">network.all_layers</font>** which contains all outputs of different layers. If you want to use L1 penalty on the activations of first hidden layer, just simply add **<font color="grey">tf.contrib.layers.l2_regularizer(lambda_l1)(network.all_layers[1])</font>** to the cost function.

```python
network.print_layers()
>> layer 0: Tensor("dropout/mul_1:0", shape=(?, 784), dtype=float32)
>> layer 1: Tensor("Relu:0", shape=(?, 800), dtype=float32)
>> layer 2: Tensor("dropout_1/mul_1:0", shape=(?, 800), dtype=float32)
>> layer 3: Tensor("Relu_1:0", shape=(?, 800), dtype=float32)
>> layer 4: Tensor("dropout_2/mul_1:0", shape=(?, 800), dtype=float32)
>> layer 5: Tensor("add_2:0", shape=(?, 10), dtype=float32)
```
For more powerful functions, please go to *[Read the Docs](http://tensorlayer.readthedocs.io/en/latest/)*.

# Easy to Modify
**<font color="grey"> Modifying Pre-train Behaviour: </font>**


Greedy layer-wise pretrain is an important task for deep neural network initialization, while there are many kinds of pre-train metrics according to different architectures and applications.

For example, the pre-train process of *[Vanilla Sparse Autoencoder](http://deeplearning.stanford.edu/wiki/index.php/Autoencoders_and_Sparsity)* can be implemented by using KL divergence as the following code, but for *[Deep Rectifier Network](http://www.jmlr.org/proceedings/papers/v15/glorot11a/glorot11a.pdf)*, the sparsity can be implemented by using the L1 regularization of activation output.

```python
# Vanilla Sparse Autoencoder
beta = 4
rho = 0.15
p_hat = tf.reduce_mean(activation_out, reduction_indices = 0)
KLD = beta * tf.reduce_sum( rho * tf.log(tf.div(rho, p_hat)) + (1- rho) * tf.log((1- rho)/ (tf.sub(float(1), p_hat))) )
```

For this reason, TensorLayer provides a simple way to modify or design your own pre-train metrice. For Autoencoder, TensorLayer uses **ReconLayer.*__*init__()** to define the reconstruction layer and cost function, to define your own cost function, just simply modify the **self.cost** in **ReconLayer.*__*init__()**. To creat your own cost expression please read *[Tensorflow Math](https://www.tensorflow.org/versions/master/api_docs/python/math_ops.html)*. By default, **ReconLayer** only updates the weights and biases of previous 1 layer by using **self.train_params = self.all _params[-4:]**, where the 4 parameters are [W_encoder, b_encoder, W_decoder, b_decoder]. If you want to update the parameters of previous 2 layers, simply modify **[-4:]** to **[-6:]**.


```python    
ReconLayer.__init__(...):
    ...
    self.train_params = self.all_params[-4:]
    ...
	self.cost = mse + L1_a + L2_w
```

**<font color="grey"> Adding Customized Regularizer: </font>**

See tensorlayer/cost.py


# Installation

**<font color="grey"> TensorFlow Installation: </font>**

This library requires Tensorflow (version >= 0.8) to be installed: *[Tensorflow installation instructions](https://www.tensorflow.org/versions/r0.9/get_started/os_setup.html)*.

**<font color="grey"> GPU Setup: </font>**

GPU-version of Tensorflow requires CUDA and cuDNN to be installed.

*[CUDA, CuDNN installation instructions](https://www.tensorflow.org/versions/master/get_started/os_setup.html#optional-install-cuda-gpus-on-linux)*.

*[CUDA download](https://developer.nvidia.com/cuda-downloads)*.

*[cuDNN download](https://developer.nvidia.com/cudnn)*.

**<font color="grey"> TensorLayer Installation: </font>**

You can install TensorLayer as follow, more detail in [Read the Docs](http://tensorlayer.readthedocs.io/en/latest/user/installation.html).

```python
python setup.py install
or
pip install . -e
```


# Ways to Contribute

TensorLayer begins as an internal repository at Imperial College Lodnon, helping researchers to test their new methods. It now encourage researches from all over the world to publish their new methods so as to promote the development of machine learning.

Your method can be merged into TensorLayer, if you can prove it is better than the existing methods. Test script with detailed descriptions is required.


# Online Documentation
The documentation is placed in [Read the Docs](http://tensorlayer.readthedocs.io/en/latest/). To generate the documentation yourself, do this:
```shell
cd docs
make html
```

# Download Documentation
You can download the documentation via [Read the Docs](http://tensorlayer.readthedocs.io/).

0. [Download PDF](https://media.readthedocs.org/pdf/tensorlayer/latest/tensorlayer.pdf)
0. [Download Epub](http://readthedocs.org/projects/tensorlayer/downloads/epub/latest/)
0. [Download HTML](http://readthedocs.org/projects/tensorlayer/downloads/htmlzip/latest/)

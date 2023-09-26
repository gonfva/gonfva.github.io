---
date: 2023-09-25T17:30:00
title: Higher loss with Tensorflow-metal, especially with ReLU
subtitle: tensorflow-metal might have an issue
tags:
  - Developer
categories: [Developer]
---

A few days ago, while working on my post about how [y=x<sup>3</sup> might be a good activation function]({{< ref "2023-09-23-x-3-as-non-linear" >}}), I discover an issue with [tensorflow-metal](https://pypi.org/project/tensorflow-metal/), the python package to use your GPU in Tensorflow on a Mac. 

I've tried to find the code for tensorflow-metal, but it seems the code is only a wrapper on a library.

I also tried to create a post on Apple's developer forums, but it doesn't allow me

![](/img/Screenshot_2023-09-26_13.31.55.png)_"You have included content in your post that is not permitted". Which content?_

I ended up writing a short message on a similar (but not exactly the same) [issue](https://developer.apple.com/forums/thread/734346)

I noticed that a set of random values gets more decimal figures in GPU than in CPU.

So with this code

```python
import tensorflow as tf   # TensorFlow registers PluggableDevices here.

with tf.device("/GPU:0"):
    tf.random.set_seed(1972)
    agpu = tf.random.normal(shape=[5], dtype=tf.float32)
    print("GPU - Random",agpu)

with tf.device("/CPU:0"):
    tf.random.set_seed(1972)
    acpu = tf.random.normal(shape=[5], dtype=tf.float32)
    print("CPU - Random",acpu)

print("equal ", tf.equal(agpu, acpu))

```
I get

```
GPU - Random tf.Tensor([-0.88528407  0.33968228 -2.0363083   1.1200726  -1.0055897 ], shape=(5,), dtype=float32)
CPU - Random tf.Tensor([-0.8852841  0.3396823 -2.036308   1.1200724 -1.00559  ], shape=(5,), dtype=float32)
equal  tf.Tensor([False False False False False], shape=(5,), dtype=bool)

```

But if I do the same in colab (Google) I get

```
GPU - Random tf.Tensor([-0.8852841  0.3396823 -2.036308   1.1200724 -1.00559  ], shape=(5,), dtype=float32)
CPU - Random tf.Tensor([-0.8852841  0.3396823 -2.036308   1.1200724 -1.00559  ], shape=(5,), dtype=float32)
equal  tf.Tensor([ True  True  True  True  True], shape=(5,), dtype=bool)
```


But the proper example appears checking with other loss functions


```python
# Lets load the data
from tensorflow.keras.datasets import mnist

(train_images, train_labels), (test_images, test_labels) = mnist.load_data()
# Now let's prepare the data into single vectors of between 0 and 1
train_images_input = train_images.reshape((60000, 28*28)).astype("float32")/255
test_images_input = test_images.reshape((10000, 28*28)).astype("float32")/255

# Now let's prepare the model
from tensorflow import keras
from tensorflow.keras import layers
import tensorflow as tf

#Set a specific seed so that things are reproducible
keras.utils.set_random_seed(1972)

@tf.function
def cubic(x):
  return (tf.math.pow(x, 3))

devices = {'gpu': tf.device('gpu:0'),'cpu': tf.device('cpu:0')}
activations = {'relu': 'relu', 'cubic': cubic}


def get_model(activation_function):
    model = keras.Sequential([
        layers.Dense(100, activation = activation_function),
        layers.Dense(10, activation = 'softmax'),
    ])

    model.compile(optimizer = 'rmsprop',
                  loss = 'sparse_categorical_crossentropy',
                  metrics = ['accuracy'])

    return model

results = []
for name_activation, activation in activations.items():
    for name_device, device in devices.items():
        with device: 
            model = get_model(activation)
            model.fit(train_images_input, train_labels, epochs=5, batch_size=128)
            test_error = model.evaluate(test_images_input, test_labels)
            results.append((name_device, name_activation, test_error))

results

```

which provide the following results

```
[('gpu', 'relu', [0.2844949960708618, 0.9228000044822693]),
 ('cpu', 'relu', [0.09160187095403671, 0.9718999862670898]),
 ('gpu', 'cubic', [0.13812969624996185, 0.9686999917030334]),
 ('cpu', 'cubic', [0.14605630934238434, 0.9715999960899353])]
```

As you can see, the code trains a model with a combination of GPU and CPU on one side, and ReLU or cubic as an activation function, on the other side.

Using ReLU, the loss is significantly higher (and the accuracy lower) when using GPU compared to using CPU. 

Using cubic, there are some differences on the results, but not so dramatic.

It seems the issue might be with precision on metal-based GPUs, but it is surprising ReLU is more susceptible.


# Keras backends

## What is a "backend"?

Keras is a model-level library, providing high-level building blocks for developing deep learning models. It does not handle itself low-level operations such as tensor products, convolutions and so on. Instead, it relies on a specialized, well-optimized tensor manipulation library to do so, serving as the "backend engine" of Keras. Rather than picking one single tensor library and making the implementation of Keras tied to that library, Keras handles the problem in a modular way, and several different backend engines can be plugged seamlessly into Keras.

At this time, Keras has two backend implementations available: the **Theano** backend and the **TensorFlow** backend.

- [Theano](http://deeplearning.net/software/theano/) is an open-source symbolic tensor manipulation framework developed by LISA/MILA Lab at Université de Montréal.
- [TensorFlow](http://www.tensorflow.org/) is an open-source symbolic tensor manipulation framework developed by Google, Inc.

## Switching from one backend to another

If you have run Keras at least once, you will find the Keras configuration file at:

`~/.keras/keras.json`

If it isn't there, you can create it.

It probably looks like this:

`{"epsilon": 1e-07, "floatx": "float32", "backend": "theano"}`

Simply change the field `backend` to either `"theano"` or `"tensorflow"`, and Keras will use the new configuration next time you run any Keras code.

## Using the abstract Keras backend to write new code

If you want the Keras modules you write to be compatible with both Theano and TensorFlow, you have to write them via the abstract Keras backend API. Here's an intro.

You can import the backend module via:
```python
from keras import backend as K
```

The code below instantiates an input placeholder. It's equivalent to `tf.placeholder()` or `T.matrix()`, `T.tensor3()`, etc.

```python
input = K.placeholder(shape=(2, 4, 5))
# also works:
input = K.placeholder(shape=(None, 4, 5))
# also works:
input = K.placeholder(ndim=3)
```

The code below instantiates a shared variable. It's equivalent to `tf.variable()` or `theano.shared()`.

```python
val = np.random.random((3, 4, 5))
var = K.variable(value=val)

# all-zeros variable:
var = K.ones(shape=(3, 4, 5))
# all-ones:
var = K.zeros(shape=(3, 4, 5))
```

Most tensor operations you will need can be done as you would in TensorFlow or Theano:

```python
a = b + c * K.abs(d)
c = K.dot(a, K.transpose(b))
a = K.sum(b, axis=2)
a = K.softmax(b)
a = concatenate([b, c], axis=-1)
# etc...
```

For more information, see the code at `keras/backend/theano_backend.py` and `keras/backend/tensorflow_backend.py`.







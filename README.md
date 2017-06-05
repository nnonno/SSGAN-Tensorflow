# Semi-supervised learning GAN in Tensorflow

## Descriptions
This is my [Tensorflow](https://www.tensorflow.org/) implementation of **Semi-supervised Learning Generative Adversarial Networks** proposed in the paper [Improved Techniques for Training GANs](http://arxiv.org/abs/1606.03498). The goal of this work is exploiting the samples generated by GAN generators to boost the performance of image classification tasks by improving generalization.

In sum, **the main idea** is training a network playing the roles of both a classifier performing image classification task as well as a discriminator trained to distinguish samples from the generator distribution from real data. To be more specific, the classifier/discriminator takes an image as input and classified it into *n+1* class, where *n* is the number of classes of a classification task. True samples are classified into the first *n* classes and generated samples are classified into the *n+1*-th class. 

The loss of this multi-task learning framework can be decomposed into the **supervised loss** 

<img src="figure/s_loss.png" height="25"/>, 

and the **GAN loss** of a discriminator

<img src="figure/gan_loss.png" height="25"/>, 

During the training phase, we jointly minimize the total loss obtained by simply combining the two losses together.

Note that this implementation only follows the main idea of the original paper while differing a lot in implementation details such as model architectures, hyperparameters, applied optimizer, etc. Also, some useful training tricks applied to this implementation are stated at the end of this README.

<!--
<img src="figure/example.png" height="200"/>.
-->

## Prerequisites

- Python 2.7 or Python 3.3+
- [Tensorflow 1.0.0](https://github.com/tensorflow/tensorflow/tree/r1.0)
- [SciPy](http://www.scipy.org/install.html)
- [NumPy](http://www.numpy.org/)

## Usage

Download datasets with:

    $ python download.py --dataset mnist cifar

Train models with downloaded dataset:

    $ python trainer.py --dataset mnist
    $ python trainer.py --dataset cifar

Test models with saved checkpoints:

    $ python evaler.py --dataset mnist --checkpoint ckpt_dir
    $ python evaler.py --dataset cifar --checkpoint ckpt_dir

Train and test your own datasets:

    $ mkdir datasets/YOUR_DATASET
    ... format your data to datasets/YOUR_DATASET ...
    ... add a data loading file datasets/YOUR_DATASET.py ...
    $ python trainer.py --dataset YOUR_DATASET --input_height h --input_width w --num_class c
    $ python evaler.py --dataset YOUR_DATASET --input_height h --input_width w --num_class c

## Results

### MNIST

* Generated samples

<img src="figure/result/mnist/samples.png" height="200"/>

## Training details

* The supervised loss

<img src="figure/result/mnist/s_loss.png" height="200"/>

* The loss of Discriminator

D_loss_real

<img src="figure/result/mnist/d_loss_real.png" height="200"/>

D_loss_fake

<img src="figure/result/mnist/d_loss_fake.png" height="200"/>

D_loss (total loss)

<img src="figure/result/mnist/d_loss.png" height="200"/>

* The loss of Generator

G_loss

<img src="figure/result/mnist/g_loss.png" height="200"/>

* Classification accuracy

<img src="figure/result/mnist/accuracy.png" height="200"/>

## Training tricks

* To avoid the fast convergence of D (discriminator) network, G (generator) network is updated more frequently.
* One-sided label smoothing is applied to the positive labels.

## Related articles
* [Unsupervised and Semi-supervised Learning with Categorical Generative Adversarial Networks](https://arxiv.org/abs/1511.06390) by Springenberg
* [Semi-Supervised Learning with Generative Adversarial Networks](https://arxiv.org/abs/1606.01583) by Odena
* [Good Semi-supervised Learning that Requires a Bad GAN](https://arxiv.org/abs/1705.09783) by Dai *et. al.*

## Author

Shao-Hua Sun / [@shaohua0116](https://shaohua0116.github.io/)

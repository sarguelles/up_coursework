# MLP and CNN on CIFAR 10  
The assignment is to build a classifier on CIFAR10 using MLP and CNN with the following specifications:
- Last layer is `Dense`
- Optimizer must be `SGD`
The solution should be implemented in a Jupyter notebook using either Keras or Pytorch.

Discussed below is a comparison of the models built, and also other observations during the exercise.

## Comparison

Below is a table comparing the accuracy reached by the two models, and the number of parameters used in order to achieve it.

Model | MLP | CNN
--- | --- | ----
Best Accuracy | 55.15% | x%
Number of Parameters | 8,759,114 | 568,490

This apppears to show the efficiency at which CNN can perform the classification task on CIFAR10 as it is able to perform better while utilizing less parameters in order to do so.

Then, looking at the hyperparameters tried out:

Hyperparameter | MLP | CNN
--- | --- | ---
batch_size | 32 | 128
epochs | 50 | 50
learning_rate | 0.001 | 0.01
momentum | 0.9 | 0.9

In this regard, a lower learning rate and batch size seems to have a better effect on the MLP model. When trying out larger learning rates (such as 0.01) batch sizes (such as 128), MLP is able to reach only around 52%; with the smaller learning rate and batch size, MLP is able to go up to 55%.

## Other Observations

Here are also some other interesting observations during the exercise. In general, deeper models seem to achieve better results, but the tradeoff is the longer training time. Also, at some point, the benefits of introducing more hidden layers might not really be that much.

### MLP

With regards to MLP, it seems to work best, at least in the case of classifying the CIFAR10 dataset, when starting off with a hidden layer that has relatively large units.

With initial hidden layers having a smaller number of units (less than 128), the accuracy becomes, on average, around 48%. Increasing the number of units in the initial hidden layer seems to help in increasing the accuracy. However, increasing the units too much in initial hidden layer (larger than 256) will likely cause a dip in the accuracy unless it the number of units are something like this [1024, 512, 256, 128, 32, 128, 256, 512, 1024] (this array describes the number of units per hidden layer).

These other layers configurations were tried out:

Configuration | Example Hidden Layer Units | Accuracy at 40 epochs
------ | ------ | ---
5-layer smaller to larger number of hidden layer units | [64, 128, 256, 512, 1024] | 48.49%
3-layer larger to smaller number of hidden layer units | [1024, 512, 256] | 49.94%
5-layer smaller to larger number of hidden layer units but with fewer units at the first hidden layer | [32, 128, 256, 512, 1024] | 50.50%
5-layer with larger units at the middle | [256, 512, 1024, 512, 256] | 50.6%
5-layer with larger units at the middle and retained number of units at the start and end | [128, 128, 1024, 128, 128] | 50.99%
3-layer same number of hidden layer units | [128, 128, 128] | 51.93%
3-layer smaller to larger number of hidden layer units | [256, 512, 1024] | 52.25%
5-layer larger to smaller number of hidden layer units with some retained units in the middle | [128, 512, 512, 1024, 2048] | 52.91%

### CNN
While achieving far better accuracy at classification than the MLP model, the CNN model could take quite a while to be trained, which was expected since it has a more complex configuration than MLP.

With CNN, it seems to work well to start off with a small number of filters, and then gradually increase it. In this case, the number of filters can be described like this [32, 32, 32, 64, 64, 64, 128, 128, 128]. Putting in more layers appears to provide better accuracy. As a comparison, when using only [32, 32, 32, 64, 64, 64], the highest accuracy that could be achieved was somewhere around 72%.

However, there is a limit to how many layers can be placed in. CIFAR10 is comprised of images with dimensions of 32 x 32, and with each convolutional layer, it is halved.

For CNN on CIFAR10, it also seems that setting the number of epochs to 30 is a fair number instead of 50.
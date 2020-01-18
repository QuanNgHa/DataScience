TensorFlow is a computational framework for building machine learning models. TensorFlow provides a variety of different toolkits that allow you to construct models at your preferred level of abstraction. You can use lower-level APIs to build models by defining a series of mathematical operations. Alternatively, you can use higher-level APIs (like tf.estimator) to specify predefined architectures, such as linear regressors or neural networks.
<img width="717" alt="Screenshot 2020-01-12 at 10 34 52 PM" src="https://user-images.githubusercontent.com/47073386/72220527-756d1c80-358c-11ea-96d0-3c170b69887b.png">

### A Quick Look at the tf.estimator API

```Python
import tensorflow as tf

# Set up a linear classifier.
classifier = tf.estimator.LinearClassifier(feature_columns)

# Train the model on some example data.
classifier.train(input_fn=train_input_fn, steps=2000)

# Use it to predict.
predictions = classifier.predict(input_fn=predict_input_fn)
```


### Epoch vs Batch Size vs Iterations
* Epoch: One Epoch is when an ENTIRE dataset is passed forward and backward through the neural network only ONCE.
* Batch Size: Total number of training examples present in a single batch.
* Iterations: Iterations is the number of batches needed to complete one epoch.

Example: We can divide the dataset of 2000 examples into batches of 500 then it will take 4 iterations to complete 1 epoch.

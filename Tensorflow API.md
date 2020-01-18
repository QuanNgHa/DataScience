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


### Common hyperparameters:
* Epoch: One Epoch is when an ENTIRE dataset is passed forward and backward through the neural network only ONCE.
* Batch Size: which is the number of training examples (chosen at random) for a single step. For example, the batch size for SGD is 1
* Steps: which is the total number of training iterations. One step calculates the loss from one batch and uses that value to modify the model's weights once.
* Iterations: Iterations is the number of batches needed to complete one epoch.
For example: We can divide the dataset of 2000 examples into batches of 500 then it will take 4 iterations to complete 1 epoch.

<img width="510" alt="Screenshot 2020-01-19 at 12 13 14 AM" src="https://user-images.githubusercontent.com/47073386/72666790-8c11e880-3a50-11ea-9204-c71a7e80d277.png">

#### Periods: 
periods, which controls the granularity of reporting. For example, if periods is set to 7 and steps is set to 70, then the exercise will output the loss value every 10 steps (or 7 times). Unlike hyperparameters, we don't expect you to modify the value of periods. Note that modifying periods does not alter what your model learns.

<img width="559" alt="Screenshot 2020-01-19 at 12 14 38 AM" src="https://user-images.githubusercontent.com/47073386/72666826-b06dc500-3a50-11ea-9d94-f145ee5906e5.png">

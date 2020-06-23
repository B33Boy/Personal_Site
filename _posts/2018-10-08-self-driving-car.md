---
title: Self Driving RC Car
layout: post
summary: A summer project where I recreated Hamuchiwa's AutoRCCcar and created my own neural network using the python library Keras.
author: Abhi Patel
date: '2018-10-08'
category: projects
thumbnail: "/assets/img/posts/side_view.jpg"
---

This summer, upon completing the Machine Learning MOOC offered by Stanford University on Coursera, I wanted to get hands-on experience through a side-project. I achieved this by recreating [Hamuchiwa's AutoRCCcar project](https://github.com/hamuchiwa/AutoRCCar) and programming my own neural network using the python library Keras. Learning Keras was greatly simplified by reading the book [Deep Learning With Python](https://www.manning.com/books/deep-learning-with-python).

### Overview
The Raspberry Pi is responsible for obtaining distance data from the sensors, and images from the PiCamera. The Raspberry Pi then sends this data to the laptop using TCP/IP. At the laptop, image processing is done and the trained deep learning model makes a prediction on what button to press in the form of a one-hot-encoded array (e.g. [0 0 1] to move right). The laptop then sends instructions to the Arduino which is interfaced with the RC controller to send the signal to move.



![sdc-configuration-diagram](/assets/img/posts/sdc-configuration.png){:class="img-fluid"}

![controller-connection](/assets/img/posts/controller.jpg){:class="img-fluid"}

![side_view](/assets/img/posts/side_view.jpg){:class="img-fluid"}

![front](/assets/img/posts/front.jpg){:class="img-fluid"}

![back](/assets/img/posts/back_view.jpg){:class="img-fluid"}

### Deep Learning
#### Data Collection and Preprocessing
With the limitations of my RC car, training was a made a lot more difficult. The turning radius of the car prevented me from training on a circular track. Thus, I had to train on a linear track, resulting in having to manually pick up the car to bring it back to the staring line. By the end of the training, I had  gathered about 1000 samples, yet it still wasn't enough. So, I used a trick I learned from the Machine Learning MOOC to generate more data by flipping all the images and labels in the x direction.

### Building The Model
The next step was to actually build the Keras model. We first split the data into training and testing sets. Our model consists of 3 Dense layers, but we add some Dropout between layers to prevent the model from picking up random patterns. The final activation is a softmax function which is necessary for our multi-label classification problem.

{% highlight python %}
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20)

# define model model = Sequential()
training_start = time.time()
print("Training data...")
model.add(Dense(100, input_dim=38400, init='uniform'))
model.add(Dropout(0.2))
model.add(Activation('relu'))
model.add(Dense(100, init='uniform'))
model.add(Dropout(0.2))
model.add(Activation('relu'))
model.add(Dense(3, init='uniform'))
model.add(Activation('softmax'))
sgd = SGD(lr=0.01, decay=1e-6, momentum=0.9, nesterov=True)
model.compile(loss='categorical_crossentropy', optimizer=sgd, metrics=['accuracy'])
# Fit the model
history = model.fit(X_train, y_train, nb_epoch=20, batch_size=1000, validation_data=(X_test, y_test))
{% endhighlight %}

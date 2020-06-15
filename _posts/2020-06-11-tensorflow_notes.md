## amazingly annoying inconsistency between v2 and v1

- v1: tf.gfile.*,   v2:tf.io.gfile.*



## tensorflow estimator
https://towardsdatascience.com/first-contact-with-tensorflow-estimator-69a5e072998d#:~:text=An%20Estimator%20is%20any%20class,as%20need%20by%20customizing%20them.
<img src="https://miro.medium.com/max/1400/1*8e8Aq_GlJFy8tGuZx1F2IA.png">
tensorflow has apis of many levels.

when learning, better start from higher level to lower level

### TensorFlow estimator overview
 - it is a high-level api that simplifies machine learning programming. 
     - automating repetitive, error-prone tasks, encapsulating best pravtices, provide a ride from training to deployment.
 - Pre-made estimator
     - an estimator is any class derived from `tf.estimator.Estimator`/
 - Estimator Interface
     - Users specify the meat of their model in `model_fn` , using conditionals to denote behaviour that differs between TRAIN, EVALUATE and PREDICT, they add a set of `intpu_fn` to describe how to handle data, optionally specifying them seperately for training, evaluation and prediction.
 - functions are consumed by the tf.estimator.Estimator class to return an initailized estimator, upon which we can call `.train, .eval .predict

 <img src="https://miro.medium.com/max/1400/1*LF9lKi-LaNRwyL5lLfKRNg.png">

 Building CNN using a estimator. please refer to
 https://towardsdatascience.com/first-contact-with-tensorflow-estimator-69a5e072998d#:~:text=An%20Estimator%20is%20any%20class,as%20need%20by%20customizing%20them.








 ## development set up

 - 06/14 bloody experience, DOn't use colab when you trying to use different tools and packages. THe version conflicts will drive you nuts. 

 In the last 3-4 days, I was having endless nightmare with constructing tensorflow model, different saved model format, tensorflow js model format. Turns out when utilizing different packages and tools the Colab environment becomes unreliable.
Because it has so many built-in packages, and the version conflicts will keep generating weirdest bugs. Better set up a local environment and do the good version control work by yourself.



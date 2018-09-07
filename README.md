# artisynth_rl
This is the code for **"Muscle Excitation Estimation in Biomechanical Simulation 
Using NAF Reinforcement Learning"** paper submitted to the Computational 
Biomechanics for Medicine XIII, A MICCAI 2018 Workshop, Granada, Spain, 
2018 September 16.

This implementation used an 
[extended version of keras-rl](https://github.com/amir-abdi/keras-rl) to train 
a reinforcement learning agent for point-to-point reaching tasks in 
biomechanical environments.  


## Dependencies

- **artisynth_core**: [ArtiSynth](https://www.artisynth.org/Main/HomePage) is a 
biomechanical modeling environment which supports both rigid bodies and finite 
elements. ArtiSynth can be downloaded from its 
[git repository](https://github.com/artisynth/artisynth_core),
and its installation guide is available 
[here](https://www.artisynth.org/Documentation/InstallGuide). 

- **keras-rl**: keras-rl is a reinforcement learning library build on top of 
the keras library. This library is extended by me to include some new 
functionalities, the details of which are available in the paper. 
Please check out the extended forked version from 
[here](https://github.com/amir-abdi/keras-rl).

- **keras**: Details available [here](https://keras.io).

- **TensorFlow**: Although keras can run with theano as its backend, we have
extensively leveraged the tensorboard features in our implementation; thus, 
we encourage users to use TensorFlow. 

- **java-json**: The message passing between the keras-rl (python) and 
artisynth (java) is through a tcp socket and the packets are sent as serialized
JSON objects. To enable json support in java, include the java-json.jar
(available in lib/ directory) in runtime libraries of the artisynth_rl project.

## Installation

- Install ArtiSynth following its 
[installation guide](https://www.artisynth.org/Documentation/InstallGuide)
- Install [TensorFlow](https://www.tensorflow.org/install/)
- Install [keras](https://keras.io/#installation)
- Checkout [keras-rl](https://github.com/amir-abdi/keras-rl)
- Checkout [artisynth_rl](https://github.com/amir-abdi/artisynth_rl)
- Include keras-rl directory as python path for your project. You can do this
either by adding the library root to the PATH environment variable, or add
the library as an external dependency in your IDE of choice.
- Add `artisynth_rl/artisynth_src` to Classpath of the artisynth project. 
If you are using eclipse, import the artisynth_rl into eclipse, 
open the **Run Configurations** for the artisynth_core project, 
switch to Classpath tab, and add the artisynth_rl project to the Classpath.

## Running

###Step 1
Run ArtiSynth with the following arguments:


    -model artisynth.models.rl.PointModelGenericRl
       [ -port 7024 -num 6 -demoType 2 -muscleOptLen 0.1 -radius 5 ] 
    -play 
    -noTimeline
where 
- `port` is the port number for the tcp socket and should 
match the port set in `src/point_model2d_naf_main.py`, 
- `num` sets the number of muscles in the model,
- `demoType` defines the dimensionality of the model and support 2 (for 2D)
and 3 (for 3D) models. In the 3D model. When `demoType` is set to 3 (3D),
`num` is ignored and the model is predefined to have 8 muscles.
- `MuscleOptLen` defines the optimal lengths of muscles at which the apply
no force on the particle.
- `radius` defines the radius of the circle on the perimeter of which
the muscles are arranged.
- `play` hints artisynth to play immediately after loading the model.
- `noTimeline` removes the timeline from artisynth as it has no use for our
reinforcement learning cause.

  
###Step 2 - Training
Run `src/point_model2d_naf_main.py` with the same environment parameters 
such as `NUM_MUSCLES`, `PORT`, and `DOF_OBSERVATIONS`. 

##Step 3 - Testing
Once the model was successfully trained (the agent was constantly reaching
the success state), call the `main` function in  `src/point_model2d_naf_main.py`
with `'test'` as input instead of `'train'` and see the results. 

##Results

Once the training is complete, the model (agent) will be able to move the 
particle by finding the correct muscle activations to reach its destination.
  
[![Point to point tracking video](https://img.youtube.com/vi/UqHt4KbsaII/0.jpg)](https://www.youtube.com/watch?v=UqHt4KbsaII) 

[![Out of domain tracking](https://img.youtube.com/vi/PQHBK3C28Q8/0.jpg)](https://www.youtube.com/watch?v=PQHBK3C28Q8)
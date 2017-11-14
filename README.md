## Autonomous vehicle navigation based on Deep Reinforcement Learning

This project is about training a vehicle, such as a car or a drone, to navigate in 3D space (either the real world, or a realistic virutal environment) autonomously based on the principle of Deep Reinforcement Learning. 

For example, a car can be trained via rewards and punishments in order acquire the skill needed to drive around all by itself for as long as possible without hitting anything. 
In other words, the car is only given high-level goals in the form of rewards (e.g., when it goes fast and long) and punishments (e.g., when it collides with anything), and it is entirely up to the car to learn the driving skills needed in order to maximize the reward.

This project is intented to be a community effort, and I am looking for volunteers to contribute to the project (see tasks below).

### Getting Started - A Basic Experiment

As of 2017.10.10 this project supports the training of a virtual car (referred to as the **Basic Car** in the following text) in a 3D AirSim environment, using the DQN deep reinforcement learning algorithm. A video showing the result after 800 episodes of training can be found [here](https://www.youtube.com/watch?v=InrQgdU8rQs&feature=youtu.b):

[![Demo Video](https://github.com/kaihuchen/DRL-AutonomousVehicles/blob/master/cardemo.jpg)](https://www.youtube.com/watch?v=InrQgdU8rQs&feature=youtu.b)

This experiment is built with the following components:

1. [The Microsoft AirSim Simulator](https://github.com/Microsoft/AirSim), which is used to provide a simple test ground in the form of a virtual suburban neighborhood with realistic physics and visuals.
2. DQN (deep Q-network) - a deep reinforcement learning method originally from Google, which was also used in DeepMind's experiments that achieved super-human results with Atari 2600 games. The actual code used in this experiment comes from [OpenAI's Baselines library](https://github.com/openai/baselines ). **Alternatively**, you can also install an enhanced version of the OpenAI Baselines from [here](https://github.com/kaihuchen/baselines), which contains added features or other enhancements.

Sensors provided to the car include only three simple depth sensors (i.e, three positive real numbers) for seeing straight ahead, and slightly to the left and right. Unlike DeepMind's Atari experiments, no raw pixels from camera are supplied to the car in this particular experiment. That'd be for another experiment in the future.

Car's throttle is fixed with no braking control, and the car is allowed only control of the steering. 

This video shows the result after 800 episodes of training, where the car is able to get out of tight corners, and run for minutes without colliding with obstacles. 

It is worth noting that:

- The car's driving style may seem erratic, and it is only because the car is not being rewarded for smooth driving at this time.
- The car will go over curbs and lawns, and it is simply because the primitive sensors provided is not sufficient to allow the car to see the differences.
- The car does not obey any traffic laws, since it is not being given sufficient sensory power to recognize anything.

### To setup this experiment:

1. Download this repository. Edit myAirSimCarClient.py so that the line **sys.path.append('../AirSim/PythonClient')** is either removed (if not needed), or is pointing to where you have installed AirSim.
2. Install [Microsoft AimSim simulator](https://github.com/Microsoft/AirSim)
3. Install [OpenAI Gym](https://github.com/openai/gym)
4. Install [OpenAI Baselines](https://github.com/openai/baselines)

The above has been tested only under Python 3.5

### To train the virtual car:

1. Configure AirSim's configuration file (settings.json) to use the Car mode. Two sample settings.json file are provided in this repository for starting AirSim in either the Car or the MultiRotor mode.
1. Start the AirSim Neighborhood environment. 
1. Start the training: execute **python3 gocar.py**. At this point you should see the car begin to train in realtime, starting with running into things very quickly, but should exhibit reasonable (but not perfect) baheavior in 800 episodes or so.

### Further experiments

The experiment above demonstrates that RL can work welll in the context of a virtual 3D realworld-like environment. There are much more work to be done, and I am **looking for volunteers** to take them on. If you manage to complete any of the following tasks, please by all means submit a pull request so that the community can share with your success.

1. **Add more depth sensors**.
The **Basic Car** (i.e., the car described in the basic experiment above) is based on only three primitive sensors, thus it has the tendency to miss skinny poles, or not seeing obstables when turning fast. The car should be able to perform better if fitted with more depth sensors (e.g., more to the front and sides).
1. **Smoother driving**.
The Basic Car drives erratically mainly because it has no sensors to measure 'smoothness', and it is not being rewarded for smoother driving. This can addressed by adding related sensors, such as G-force sensor, tilt sensor, etc., plus appropriate rewarding formula.
1. **Reload and continue training**.
For the purpose of long term research, it is imperitive  to be able to reload a model from the last training session, and then continue with the training. Long term training often requires intensive training in certain narrow areas that requires continuing the training fro a previous session. For example, after the Basic Car acquires the basic driving skill, it usually does well on the open streets but no so much in tight corners. If left unattended the training tends to take longer and longer to converge, since it takes a long time for the car to get to places with tight corners where the car can learn something new. One way to address this is to identify a number of training grounds useful for the purpose, then continue the training session specifically on such intensive training grounds. This cannot be achieved without the "continue training" capability.
As of this writing the deepq method in OpenAI Baselines does not support a way to reload a previously trained model to continue with training. There has been some [possible solution](https://gist.github.com/Sohojoe/25fe35364eba0ed0d3fb70b1e6068dcb) mentioned but needs to be confirmed. 
1. **Support multi-dimensional actions**.
The basic car is allowed to control only the steering, but nothing else (such as throttle, brake, etc.). It is preferrable that it can control many more, but  OpenAI's deepq currently does not support it. We either need to improve deepq to make it support multi-dimensional action space, or try out other RL algorithms.
1. **Navigate with raw pixels**.
The original DQN was trained to play the Atari 2600 games with nothing but raw pixels as its input. We designed the Basic Car with only a number of simple depth sensors as a way to get something working first. It would be great to upgrade the Basic Car to learn from the raw pixels as well.
1. **Staying on the road**.
The Basic Car has the tendency to drive all over the place, including going over curbs and grassy areas.
This is because its primitive sensors won't allow it to distinguish between roadway, curbs, and grasses. The goal for this experiment is to keep the car strictly on the road by giving it access to the camera and segmentation map, and train it to avoid curbs and grasses.
1. **Flying drones**.
Applying the enhancements above to fly an AirSim Multirotor drone.
1. **Virtual Pet mode**. Add keyboard reward/penalty commands, so that a user is able to apply reward or penalty when justified. Here it is entirely up to the user to decide when to apply rewards/penalties. The challenge here is that invariably there will be delay in the application of the rewards/penalties.
1. **Following traffic laws (to some degree)**.
1. **Applying trained models to a real car or drone**.
1. **Real Pet mode**. Combine a real vehicle with vision and the Virtual Pet mode mentioned above, and train the vehicle to navigate towards people, to favor happy facial expressions from people, and to recognize the identify of people.

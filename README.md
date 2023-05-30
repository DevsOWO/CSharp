# OWO SDK Documentation
OWO is a new haptic technology that allows developers to add the sense of touch to their games. Using OWO, users can feel in real-time when playing videogames. They can feel sensations like the wind, a shot, a hug and many more. 
Adding OWO’s SDK to a game is very simple: 
1. Establish the communication: Game - OWO App.
2. Choose a sensation or create it.
3. Send it to one or more muscles.
4. #Feelthegame.  

# Connection
There are two ways to establish a connection with OWO: 

**1. Manual connection:** The user must enter the IP that the OWO App provides during the games scan. To establish this connection you have to use the ``Connect`` method of the ``OWO`` class.
```
//The ip is provided to the user by the owo app.
OWO.Connect(ip);
```
It is also possible to manually connect to multiple ips:
```
OWO.Connect(ip1, ip2, ip3...);
```
**2. Automatic IP:** Through the ``AutoConnect`` method, the ``OWO`` class will begin to search for the OWO App through the local network and it will establish a connection. This may not work if there is an App like OpenVPN opened or if the network does not allow this type of connection (broadcast).
```
OWO.AutoConnect();
```
**3. IP Scanning:** By calling the ``StartScan`` method, the API will start searching for nearby owo apps and store the results in the ``DiscoveredApps`` property of the ``OWO`` class.
```
OWO.StartScan();

//... wait some time

OWO.Connect(OWO.DiscoveredApps);
```

# Add sensations to your project
OWO has created over 30 different sensations for you to use in your game. There are many different types of sensations that we have divided in: impacts, interactions, experiences and alerts. You can:
1. Use OWO’s previously created sensations.
2. Develop your own sensations to create your own experience.

We invite you to use your imagination, don’t just stick to the name we give each sensation, be creative and maybe you’ll find new ways to apply the existing sensations!

# How to send sensations
Once the connection is established, you can send sensations using the ``Send`` method.
```
OWO.Send(Sensation);
```
## How does a sensation work?
A sensation is composed of one or more microsensations. A microsensation is the base, the smallest unit users can calibrate. To create micro, you have to modify 6 different parameters:
1. **Frequency:** The frequency is the number of pulses per second the microsensation sends to the player.
    - A micro with frequency equal to 1hz will send one pulse per second, which would feel like independent impacts (e.g. dart).
    - A micro with a high frequency, 100hz for example, will result in a “continuous” sensation (e.g. wind, bleeding, etc.).
    
2. **Duration:** The amount of time (in seconds) that the microsensation will last. For example, if you create a sensation with Frequency: 1Hz; Duration: 1 sec, you will feel a dart after 1 second. On the other hand, if you create a sensation with Frequency: 10Hz; Duration: 0.1 sec, you will immediately feel a dart.

> You cannot establish the frequency of 1Hz in less than 1 sec.

3. **Intensity percentage:** Each user calibrates their maximum intensity level (100%) during their calibration process in the OWO App. You can assign the intensity percentage that users will feel. E.g: 50% of the users calibration, 75%, etc.

4. **Fade in:** How long it takes for the microsensation to progressively go from 0 to the assigned intensity percentage (in seconds), the maximum is 2 seconds. 

5. **Fade out:** How long it takes for the microsensation to progressively go from the assigned intensity percentage to 0 (in seconds), the maximum is 2 seconds.

6. **Exit time:** The additional time in seconds between one microsensation and the next one to play.

## How to create your own sensations
You can create sensations in several ways:
1. Create a sensation and save it in a variable:
```
var ball = SensationsFactory.Create(100,0.1f,100,0,0,0);
```
2. Create a sensation, then assign muscles:
```
var ballWithMuscles = ball.WithMuscles(Muscle.Pectoral_R);
```
3. Create a sensation, then assign muscles with overrided intensity:
```
var softBall = ball.WithMuscles(Muscle.Pectoral_R.WithIntensity(50));
```
4. Create a sensations sequence:
```
var sequence = daggerEntry.Append(bleeding);
```
5. Parse a sensation from a raw string:
```
var ball = (Sensation)"100,1,100,0,0,0,Ball";
```
# Game configuration
## Baked sensations
Baked sensations are those with an unique identifier, can be created from the sensations creator or by code. The sensations exported from the sensations creator are also baked sensations.
```
ball.Bake(id, "Ball"); //This results in a baked sensation with the name Ball.
```
Once a sensation is baked, it cannot be modified. 

**Important:** To be able to use a baked sensation, first it is necessary to include it in the GameAuth of your game (explained below).

## Game Auth
The game auth contains the information of the baked sensations included in the game. These sensations will be shown to the user in the OWO application and will allow them to modify their intensity.
```
var auth = GameAuth.Create(bakedBall, bakedKnife...);
```
To add a sensation to the game auth, first it is necessary to bake it:
```
var bakedBall = BakedSensation.Parse("0~Ball~59,1,90,0,0,0,|0%100~impact-0"); //Sensation exported from the Sensations Creator
```
Once the game auth is ready, call the ```Configure``` method of the ```OWO```.
```
OWO.Configure(auth);
```

# Develop new sensations
You can use the sensations designed by the OWO team or create your own sensations by modifying five parameters.

### Sensations Creator
Newest version: [Sensations Creator](https://drive.google.com/drive/folders/1dn2ZuG3ZY9Vl2gHHQyiOQZO3saRcux9k?usp=sharing).

The sensations creator is a tool with which you can experiment and create your own sensations.

### OWO Visualizer
Newest version: [OWO Visualizer](https://drive.google.com/drive/folders/1hTsusIT3jVPqzCBHcwuw6HUxrHbDdwXG?usp=sharing).

With the OWO visualizer you can see, in real time, what the user will be feeling while playing your game without using the OWO Skin.


If you have any questions, contact us! We'll be happy to answer them. 
You can send us an email to devs@owogame.com or join our [Discord server](https://discord.gg/kCVkN4nrW7). 
We look forward to chatting with you!

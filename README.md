# Flapper
### A Raspberry Pi controlled tool to help deal with crazy dogs!

## Background
We have three dogs. They pretty much go crazy any time someone comes to the door, jumping on the person when they enter. While one of the pups was in obedience training we brought up this issue. Her suggestion was to have some good outside the door and, when we were entering, to say "AWAY!" and toss some to a place such that the dogs would move away from where we were to get the food. Due to the layout of our entrance that proved to be somewhat impractical, so I decided to geek it up a notch!

I thought that it would be cool to be able to use a remote to trigger some gadget to drop a bit of food that would, in turn, redirect the crazy pups away from us. It couldn't be an infrared remote because we would want to potentially trigger this from outside the door. Some quick Googling yielded very inexpensive 315MHz keyFOB and receiver pairs, and I already knew that small servos would be similarly cheap. So, a geeking I went!

## The Thought Process

My first thought was to find something already existing that would act as the hopper for the food and either had a hinged lid, or one that I could modify to be hinged. I thought about using Mason jars, and found a product that gave me the hinged lid I was seeking. However, that part could only be found & purchased online, and with delivery would cost more than the Raspberry Pi Zero W that I was using to run the whole show! My next thought was to use some sort of plumbing valve, and that search led me to check valves. There were several candidates, but again the cost was prohibitive for what I was doing.

I eventually settled on good old ABS, using a 1-1/2" 45 degree elbow. Initially, I thought I'd reuse a platic container for the hopper and the first iteration of Flapper used a Vitamin Water bottle. With a bit of elbow grease I was able to fit the lid of the bottle into one end of the 45° elbow, so we were off to the races! The next step was hooking up the servo and some sort of flap.

There were several ways I could approach the flap, but during a trip to the local hardware store I was inspired by a simple door stop. It's a half sphere type that you mount on the wall to protect it from a door handle. It's a rubber-like material, meaning it was easy to work with, and would close the mouth of the flapper quite effectively. I splurged & bought it for $1.99! After digging through all the bits & pieces of hardware I've collected over many years, I found a small hinge that would fit my setup nicely. I also found a small, plastic mounting bracket for window blinds, that I had kept for no particular reason for the last 25 years, that would function as the connection point on the flapper itself.

## Iteration One

Once I had all the hardware in place, I wrote up some python code to test opening the flap with the servo. There were plenty of examples from which to base my code, so I quickly had something in place using the Pi's GPIO library PWM class/module. I was able to toggle the servo between 0° and 90°, which would open the flap sufficiently to drop some food.

A quick "live fire" test proved that the concept work, and I had some happy dogs in my office! The developer in me was ready to package the whole thing up and ship it, but the tester in me wanted to try a couple more times. Lo and behold, the food got stuck in the neck of the Vitamin Water bottle as it narrowed to the opening. I was going to have to rework that aspect of the Flapper.

Before I did, though, I decided to test out the 315 MHz and receiver. Again, there was plenty of help online and I put that code together with the servo-controlling code. Bzzzzzt! The previously working servo became jittery and wandered to points not conducive to keeping the feed in the hopper. The culprit was having to manually code a `timer.sleep()` call in both the servo controller and the receiver. The PWM didn't care where that sleep call originated, the servo would use it! I was able to work around the issue by putting the servo code into another python source file and using `subprocess` to call it, but that was a nauseatingly hacky approach.

## Iteration Two

After more research, the solution was to use a dedicated servo controller board with the Pi Zero. The one I bought was relatively inexpensive, but certainly was overkill by providing 16 servo channels. However, it would move the handling of the servo control signal away from the Pi itself.

I also fixed the issue with the food sticking the neck of the bottle by replacing it with just a short piece of ABS pipe. There was now literally no bottleneck for the food to flow out!

I updated the code to use the controller board's library, which made things dramatically simpler on my end. I was also able to bring the two source files back together into a single one, `flapper.py`.

## Production

Some quick tests, with associated happy dogs, and we were in business. My only tweak required was to replace the thin twist tie that I had used to connect the servo to the flapper with a piece of 14 gauge copper wire to provide enough stiffness when closing the flap against the flow of food.

The Flapper is now in place and ready to teach the dogs to run into the kitchen any time we say "AWAY!!"



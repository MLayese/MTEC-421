##MTEC-421 3D Game Kit Final
#### Myron Layese

##### Professor Jean-Luc Cohen
--
 
### Intro - It worked until it didn't...

For this project we had the task of creating the sounds for a project of our choosing. I used the 3D Game Kit from the Unity Asset Store. Everything went well! Until the very last day where I can't even load Unity anymore (no really, it crashes). Anywho, it should run on other people's computers. I'll do documentation for what I did.

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-10%20at%208.54.42%20PM.png)

--
#SetUp
Before tackling the game I set up the Wwise project with all the intended functions. I also organized all my sounds into a designated folder for easy access.

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-13%20at%201.14.08%20AM.png)

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-12%20at%2011.56.23%20PM.png)

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-12%20at%2011.56.30%20PM.png)

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-12%20at%2011.56.56%20PM.png)

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-12%20at%2011.56.13%20PM.png)

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-12%20at%2011.56.19%20PM.png)

#Audio
While running around "Level 1" I made sounds for the main character, enemies, destroyable objects, ambiance, etc. You can listen to them here:


####Ellen
I spent the most time on Ellen's sounds. I made different noises for her footsteps depending on the type of material she was on and had a switch case to render each noise. As of now there's SFX for grass, dirt, and puddles. 

The footsteps were the hardest part to implement. I used a debug log to see what kind of material she was on and used a renderer to refrence the materials. I made a script that switched her footsteps based on the type of material she was on.

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-12%20at%203.31.09%20PM.png)

To flesh out her character there's SFX for her jumping, attacking, getting damaged, and landing animations. I used "PostEvent" for these in the player controller script because each animation had it's own seperate function. It was very easy to implement. 

I also made an RTPC for her hitpoints that would trigger a "mid health" and "low health" state. Starting at three hearts she would hear her own heart beat and as she loses more health, a low pass filter would come into the audio mix, dulling out her hearing. 

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-13%20at%201.08.44%20AM.png)

Something I didn't realize was that I had to convert the ```currentHitPoints``` from the health canvas to a float. I spent a good ten minutes wondering why the RTPC wasn't adjusting to her lack of heath. Making a float called ```ow``` and having equal her ```currentHitPoints``` fixed this problem.

For her weapon I mixed together the sounds of a lightsaber, some impacts "swooshes" and electric crackling. I just thought it'd be neat to give it more "oomph" so the low frequencies in the lightsaber really helped.  There was some light compression and reverb added as well, y'know how it goes. 

The weapon sounds are triggered by the animation events rather than just using "GetInputKey". It makes the sounds more coherent and prevents the user from accidentally triggering weapon sounds before they acquire her staff.

####Chomper
For the "Chomper" there were three sounds: The "Attack", the "Hurt", and the "Impact" sounds. 

The "Attack" sounds are triggered by the Attack animation event, at the frame where they jump up to bite. Those sounds were made from an old Sample Library I had. I mixed together sounds from a few reptiles to make them sound "slimey". 

The "Hurt" and "Impact" sounds are triggered when the ragdoll animation is played. I saw in the scripts that when they run out of health to transform into the a ragdoll gameobject. I added a "PostEvent" for each sound in thge void Awake.

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-13%20at%201.09.17%20AM.png)

I also made a script for intermittent emitters that could be triggered on collision. I planned on using them for the chompers but couldn't sadly.
I was going to add more to them like footsteps but I can't open the Unity file anymore. Bummer. 

####Ambiance
I placed an Audio Emitter script on a bunch of gameobjects. The Dropship, the various fireflies, water, etc. It made it feel more immersive rather than just having a global ambiance track that played through the entire game. 

![](https://raw.githubusercontent.com/MLayese/MTEC421Final/main/Screen%20Shot%202022-12-13%20at%201.06.11%20AM.png)

I looped whatever ambient sounds I wanted and posted that in the respective event. I added a seek function to the event just in case the ambient noise was triggered again somehow. This prevents multiple iterations starting at the same time, especially when the player walks into the attenuation radius over and over. Once the player leaves the radius there's a "stop" event that is called. This allowed for more seamless transitions in the enviroment. 

I used this method for the music as well - calling it using the Emmiter script. It would trigger different states like when the player is near an objective. The music would get louder while dulling out the ambiance. I saw that this was a good way to get the player's attention. The first portion of the level doesn't have any music. It's only after the first health box that you can hear music, beckoning you towards the contraption that opens the gate.
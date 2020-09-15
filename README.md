# Unity-Object-Fade

### Info:
These scripts make objects that are directly between the camera and the player transparent so the player is always in view. Currently, there are four modes:
###
Single: Only makes this object transparent when between the camera and the player.
###
Whole: Makes this object, all siblings, and all parents transparent (does not affect childs or childs of siblings).
###
Parent: Makes this object and the parent object transparent.
###
Multiple: Makes this object and other selected objects transparent.

#

### How to use:
Download and add all scripts to your Unity project. Add the FadeCheck script to your player object, and add the ObjectFade script to any object you want to turn transparent when
blocking the camera's view of the player.
The ObjectFade script looks for a Game Object called Player by default. If your player object is not called Player, you can change it from the ObjectFade script on line 27:
fadeCheck = GameObject.Find("Player").GetComponent<FadeCheck>();
From the FadeCheck component on your player object, you can set the global value of what the alpha of the object should be when made transparent using the Fade To slider.
From the ObjectFade component on the objects you want to fade, you can set the Fade Type, override the global alpha value by checking the Custom Fade box and setting the Fade To
slider, and set which objects the Multiple Fade Type makes transparent and if this Fade Type should go by the object itself or the parent object when checking what is between
the player and the camera.
The MaterialObjectFade script does not need to be attached to any Game Object. It just needs to be in the project.
  
#
  
### How it works:
A ray is cast from the main camera to the player object. If any object blocks this ray, then it is blocking the camera from seeing the player. The FadeCheck script checks to see
if the object that is blocking the view of the player has the ObjectFade script attached to it. If it does, it then checks to see if that object has the Fade Type set to Whole. If
it is Whole, then the FadeCheck script checks to see if that object has a parent. If it does, it checks to see if that parent has a parent. And so on, until there is no parent
above the last parent it found. It sets that object as the Object Hit, instead of the object that it actually hit. If the object Fade Type is Single, or the object doesn't have
the ObjectFade script attached, it sets the object it actually hit as the Object Hit.
The ObjectFade script checks to see what object the FadeCheck script is hitting. If the Fade Type is set to Single, then the object will become transparent when the Object Hit
in the FadeCheck script is the same that the ObjectFade script is on. This is done by going through all materials attached to the object's renderer, changing their render
type, and setting the alpha value of the materials color to the global alpha value, or a custom one if that is set.
If the Fade Type is set to whole, it instead checks if the Object Hit from the FadeCheck is the same as its "Highest Parent". If it is, the ObjectFade script runs through every
sibling and parent, starting from the object that the ObjectFade script is attached to, and makes the objects transparent using the same method above.
If an object with the ObjectFade script is set to Faded, but the Object Hit from the FadeCheck script is no longer the object it was looking for, it reverses the action, setting
all materials back to opaque and setting the alpha value of the materials color to 1.

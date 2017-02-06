# Roll a Ball

We want to be able to pick up our collectable game objects when our player game object collides with them.
To do this we need to detect our collisions
between the player game object and the PickUp game objects.
We will need to have these collisions trigger a new behaviour and we will need to
test these collisions to make sure we are picking up the correct objects.
The PickUp objects, the player's sphere, the ground plane and the walls all have colliders that inform us about collisions.

If we didn't test our collisions to find out which objects we have collided with we could collect the wrong objects.
We could collect the floor, or the walls.
As a matter of face if we didn't test our collisions
on the very first frame of the game we would
come in contact with the ground plane
and we would collect the ground plane
and then we would fall in to the void of the scene space
and the game would essentially be over.
First, we don't need our player to remain inactive.
so let's tick the Active checkbox and
bring back our player.
Next let's select the PlayerController script
and open it for editing.
Just a note, we could edit this
script regardless of whether the game
object is active or not.
Now that we have the script open,
what code are we going to write?
We could write collider
and then search the documentation using the
hot key combination.
But there is a different way that we could do this as well.
Let's return to Unity and look at the details
of our player game object.
What we are interested in here is the
sphere collider component.
In the header of each component on the left
is the component's turndown arrow,
the icon, the Enable checkbox
if it's available, and the type of the component.
On the right is the context sensitive gear gizmo
and an icon of a little book with a question mark.
Now this is what we need.
This is the quick link to the component reference.
If we select this icon we are taken
not to the scripting reference but to the
component reference.
We would read this document to find out more
about how to use this component in the context
of the editor.
We, however, want to find out how to
script to this component's class.
To do this we simply switch to scripting
and we are taken to the scripting reference
for the sphere collider.
We want to detect and test our collisions.
For this project we are going to use
OnTriggerEnter.
Just imagine if we were, say, a daring plumber
and we jumped up to collect a perfect arch of coins
and bounced off the very first one as we
collected it and fell back to the ground.
Not very elegant.
This code will give us the ability to detect
the contact between our player game object
and our PickUp game objects
without actually creating a physical collision.
The code example however is not exactly
what we're looking for.
But that's okay, we can change that.
First, let's copy the code,
and then let's return to our scripting application.
Now that we're back in scripting let's paste the code.
As we have copied this code from a webpage
let's correct the indents.
In this case I'm going to make sure the
indents are tabs and that all of the tabs are
correctly aligned.
Next, let's look at this code.
We are using the function OnTriggerEnter.
OnTriggerEnter will be called by Unity
when our player game object first touches
a trigger collider.
We are given as an argument
a reference to the trigger collider that we have touched.
This is the collider called Other.
This reference gives us a way to get a
hold of the colliders that we touch.
With this code, when we touch another trigger
collider we will destroy the game object
that the trigger collider is attached to
through the reference other.gameObject.
By destroying that game object, the game object,
all of it's components and all of it's
children and their components are removed
from the scene.
For the sake of example in this assignment
we won't destroy the other game object
we will deactivate it.
Just like we deactivated the player object
when we were creating our PickUp objects.
First, let's remove the Destroy(other.gameobject)
code from the function.
But let's paste it down below our script
as a palette to work with.
How will we deactivate our PickUp objects?
Well what clues do we have?
We can address the other collider's game
object through other.gameObject.
We can see this here in the sample code.
And we want to test the other game object
and if it's a PickUp object we want to deactivate
that game object.
So let's look up GameObject
with our hot key combination and see what we can find.
Now we have the page on GameObject.
There are two important items here that we want.
They are tag,
tag allows us to identify the game object
by comparing the tag value to a string.
And SetActive.
This is how we activate or deactivate a game object through code.
The last item we need to know about is
Compare Tag.
Compare Tag allows us to efficiently
compare the tag of any game object to a sting value.
Let's open up these three items, each in their own tab.
Tag allows us to identify a game object by a tag value.
We must declare our tags in the Tags and Layers Panel before using them.
It is possible to test a tag against a string value directly
with code like
if gameObject.tag is the same as some string value.
But there is a more efficient built-in way to do this,
and that is CompareTag
With CompareTag we can efficiently
compare the tag of any game object with a string value.
Let's copy the sample code and
paste it in to our working palette.
Now GameObject.SetActive.
This is how we activate or deactivate a game object.
This is the code equivalent of clicking
the Active check box next to the
Name field in the Inspector.
In our case, just like the code snippet,
we will call GameObject.SetActive (false)
to deactivate our PickUp game objects.
Let's copy this code and returning to our script editor
paste it in to our palette as well.
I feel we have enough pieces to write our code
so let's write if (other.gameObject.CompareTag
with the string value of PickUp
and we will have to define this tag in Unity later.
other.gameObject.SetActive (false);
Now this code will be called every time
we touch a trigger collider.
We are given a reference to the collider we touch,
we test it's tag,
and if the tag is the same as the string value
PickUp we will take the other game object
and we will call SetActive (false),
which will deactivate that game object.
Now we don't need this code we've been saving anymore
and keeping it will only confuse the compiler
so we can delete it.
Let's save this script and return to Unity
and check for errors.
The first thing we need to do it set up the tag value
for the PickUp objects.
Select the prefab asset for the PickUp object.
When we look at the tag list
we don't see any tag called PickUp
so we need to add one.
Select Add Tag.
This brings up the Tags and Layers Panel.
Here we can customise tags and layers.
Note that this list is empty.
To create a new custom tag select the + button
to add a new row to the tags list.
In the new empty element, in our case tag 0,
type PickUp, and this is case sensitive
and needs to be exactly the same string
that we have in our script.
If in doubt we can copy and paste
this string to get the exact value.
When we look back at the prefab asset
note that the asset is still untagged.
By selecting Add Tag we changed our focus
from the prefab asset to the Tag Manager
and in the Tag Manager we created our tag.
Now we need to apply that tag
to the prefab asset.
Select the Tag drop down again
and see how we now have PickUp in the list.
Select this tag from the list and the asset
is now tagged PickUp.
And with the power of prefabs
all of the instances are now tagged PickUp as well.
Now let's test our game.
Save the scene and enter play mode.
Hmm, okay, our tag is set to PickUp
but we are still bouncing off the PickUp cubes
just like we are bouncing off the walls.
So let's exit play mode.
Before we discuss why we are bouncing off
the PickUp cubes rather than picking them up
we need to have a brief discussion about
Unity's physics system.
I'm going to enter play mode for this discussion.
Let's look at one of our cubes and our player.
As an aside we can select two or
more game objects at the same time
and inspect them all.
We do this by holding down the command key
on the mac or the control key on the PC,
and selecting the game objects.
When we select multiple game objects
note how the inspector changes to show
the common components and property
values of the selected game objects.
The inspector also allows for multi-object editing.
Using multi-object editing I'm going to
disable the mesh renderer on both the
player's sphere and the selected cube with a single click.
This leaves us with the two green outlines
of the collider volumes for these two objects.
How do collisions work in Unity's physics engine?
The physics engine does not allow two collider
volumes to overlap.
When the physics engine detects that any two
or more colliders will overlap that frame
the physics engine will look at the objects and
analyse their speed and rotation and shape
and calculate a collision.
One of the major factors in this calculation
is whether the colliders are static
or dynamic.
Static colliders are usually non-moving
parts of your scene, like the walls, the floor,
or the fountain in the courtyard.
Dynamic colliders are things that move
like the player's sphere or a car.
When calculating a collision the static geometry
will not be affected by the collision.
But the dynamic objects will be.
In our case the player's sphere is dynamic,
or moving geometry, and it is bouncing
off the static geometry of the cubes.
Just as it bounces off the static geometry
of the walls.
The physics engine can however allow the
penetration or overlap of collider volumes.
When it does this the physics engine
still calculates the collider volumes and
keeps track of the collider overlap.
But it doesn't physically act on the overlapping
objects, it doesn't cause a collision.
We do this by making our colliders in to
triggers, or trigger colliders.
When we make our colliders in to a trigger,
or trigger collider, we can detect
the contact with that trigger through the
OnTrigger event messages.
When a collider is a trigger you can do
clever things like place a trigger in the middle
of a doorway in, say, an adventure game,
and when the player enters the trigger
the mini-map updates and a message plays
'you have discovered this room'.
Or every time your player walks around
that corner spiders drop from the ceiling
because the player has walked through a trigger.
For more information on OnCollision and on
trigger messages see the lessons linked below.
We are using OnTriggerEnter in our code
rather than OnCollisionEnter.
So we need to change our collider volumes
in to trigger volumes.
To do this we must be out of play mode.
Let's select the prefab asset and look at
the box collider component.
Here we select Is Trigger
and again the power of prefabs
all of our PickUp objects are now using trigger colliders.
Let's save our scene, enter play mode and test.
And as our player enters the trigger
we pickup the objects.
Excellent.
Let's exit play mode.
Everything looks great.
We only have one issue.
We have made one small mistake,
and this is related to how Unity
optimises it's physics.
As a performance optimisation Unity
calculates all the volumes
of all the static colliders in a scene
and holds this information in a cache.
This makes sense as static colliders
shouldn't move, and this saves recalculating this
information every frame.
Where we have made our mistake is by rotating our cubes.
Any time we move, rotate, or scale a static collider
Unity will recalculate all the static colliders again
and update the static collider cache.
To recalculate the cache takes resources.
We can move, rotate or scale dynamic
colliders as often as we want and Unity won't
recache any collider volumes.
Unity expects us to move colliders.
We simply need to indicate to Unity which
colliders are dynamic before we move them.
We do this by using the rigid body component.
Any game object with a collider
and a rigid body is considered dynamic.
Any game object with a collider attached
but no physics rigid body is expected to be static.
Currently our PickUp game objects have a
box collider but no rigid body.
So Unity is recalculating our static
collider cache every frame.
The solution is to add a rigid body
to the PickUp objects.
This will move the cubes from being static colliders
to being dynamic colliders.
Let's save and play.
And our cubes fall through the floor.
Gravity pulls them down, and as they are triggers
they don't collide with the floor.
Let's exit play mode.
If we look at the rigid body component
we could simply disable Use Gravity,
which would prevent the cubes from being pulled downwards.
This is only a partial solution however.
If we did this, even though our cubes
would not respond to gravity they would still
respond to physics forces
There is a better solution.
And that is to select Is Kinematic.
When we do this we set this rigid body component to be
a kinematic rigid body.
A kinematic rigid body will not react
to physics forces and it can be animated
and moved by it's transform.
This is great for everything from objects with colliders
like elevators and moving platforms,
to objects with triggers, like our collectables
that need to be animated or moved by their transform.
For more information on the rigid body
and Is Kinematic see the lessons linked below.
Let's save our scene and enter play mode to test.
Now our behaviour is identical and performant.
So, static colliders shouldn't move,
like walls and floors.
Dynamic colliders can move,
and have a rigid body attached.
Standard rigid bodies are moved using physics forces.
Kinematic rigid bodies are moved using
their transform.

In the next assignment we will count our collectable object and make a simple UI to display our score and messages.


### [Displaying the Score and Text](https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial/displaying-score-and-text?playlist=17141)

### [Build the Game](https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial/building-game?playlist=17141)
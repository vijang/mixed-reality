---
title: Direct manipulation
description: Overview of the direct manipulation input model
author: caseymeekhof
ms.author: cmeekhof
ms.date: 04/02/2019
ms.topic: article
keywords: Mixed Reality, Gaze, gaze targeting, interaction, design
---

# Direct manipulation
Direct manipulation is an input model that involves touching holograms directly with your hands. The goal with direct manipulation is that objects behave just as they do in the real world. Buttons can be activated simply by pressing them, objects can be picked up by grabbing them, and 2D content behaves like a virtual touchscreen.  Because of this, direct manipulation is easy for users to learn, and it's fun too.  It is considered a "near" input model, meaning it is best used for interacting with content that is within arms reach.

A key ingredient that makes direct manipulation easy to learn is that it is affordance-based. There are no symbolic gestures to teach users. All interactions should be built around a visual element that can be touched or grabbed.

## Device support

<table>
    <colgroup>
    <col width="25%" />
    <col width="25%" />
    <col width="25%" />
    <col width="25%" />
    </colgroup>
    <tr>
        <td><strong>Input model</strong></td>
        <td><a href="hololens-hardware-details.md"><strong>HoloLens (1st gen)</strong></a></td>
        <td><strong>HoloLens 2</strong></td>
        <td><a href="immersive-headset-hardware-details.md"><strong>Immersive headsets</strong></a></td>
    </tr>
     <tr>
        <td>Direct manipulation (Near hand interaction)</td>
        <td>❌ Not supported</td>
        <td>✔️ Recommended</td>
        <td>➕ An alternate option but <a href="point-and-commit.md">Point and commit (far interaction)</a> is recommended</td>
    </tr>
</table>

Direct manipulation is a primary input model on HoloLens 2, utilizing the new articulated hand tracking system. The input model is also available on immersive headsets through the use of motion controllers, but is not recommended a primary means of interaction outside of object manipulation.  Direct manipluation is not available on HoloLens v1.

## Collidable fingertip
On HoloLens 2, user's real hands are recognized and interpreted as left and right hand skeletal models. To implement the idea of touching holograms directly with hands, ideally, 5 colliders could be attached to 5 fingertips of each hand skeletal model. However, practically, due to the lack of tactile feedback, 10 collidable fingertips cause lots of unexpected and unpredictable collisions with holograms. Hence, we suggest to only put a collider on each index finger. The collidable index fingertips can still serve as active touch points for diverse touch gestures involving other fingers, such as 1 finger press, 1 finger tap, 2 finger press and 5 finger press.

![Collidable fingertip image](images/Collidable-Fingertip-720px.jpg)<br>

### Sphere collider
Instead of using random generic shape, we suggest to use a sphere collider and to visually render it to provide better cues for near targeting. The sphere's diameter should match the thickness of the index finger to increase touch accuracy. It will be easy to retrieve the variable of finger thickness by calling the hand API.

<br>

### Fingertip cursor
In addition to rendering a collidable sphere on the index fingertip, we create an advance solution, fingertip cursor, to achieve better near targeting experience interactively. It is a donut shape cursor attached on the index fingertip. According to proximity, it dynamically reacts to a target in term of orientation and size as below:
* When an index finger moves toward a hologram, the cursor is always parallel to the surface of the hologram and gradually shrinks its size accordingly. 
* As soon as the finger touch the surface, the cursor shrinks into a dot and emits a touch event.

<br> With the interactive feedback, users can achieve high precision near targeting tasks, such as triggering a hyperlink on a web content or pressing a button. <br>

![Fingertip cursor image](images/Fingertip-Cursor-720px.jpg)<br>

## Bounding box with proximity shader
The hologram itself also requires to provide both visual and audio feedbacks to compensate the lack of tactile feedback. For that, we generate the concept of bounding box with proximity shader. A bounding box is a minimun volumetric area that encloses a 3D object. The bounding box has an interactive rendering mechanism called proximity shader. The proximity shader behaves as below:

* When the index finger is within a range, a fingertip spotlight is cast on the surface of bounding box. 
* When the fingertip gets closer to the surface, the spotlight condenses accordingly. 
* As soon as the fingertip touch the surface, the whole bounding box changes the color or generate visual effect to reflect the touch state. 
* Meanwhile, a sound effect can be activated to enhance the visual touch feedback.

![Bounding box with proximity shader image](images/Bounding-Box-With-Proximity-Shader-720px.jpg)<br>

## Pressable button
With a collidable fingertip, users are now ready to interact with the very fundamental holographic UI component, pressable button. A pressable button is a holographic button tailored for direct finger press. Again, due to the lack of tactile feedback, a pressable button equips a couple mechanisms to tackle tactile feedback related issues. 
* The first mechanism is bounding box with proximity shader, which has already been addressed in the foregoing paragraph. It serves to provide better sense of proximity for users to approach and make contact with a button. 
* The second one is depression. It creates sense of press, after a fingertip contacts the button. The mechanism is that the button tightly moves with the fingertip along the depth axis. The button can be triggered as soon as reaching a designated depth (on press) or leaving the depth (on release) after passing through it. 
* The sound effect should be added to enhance feedback, when the button is triggered. 

![Pressable button image](images/Pressable-Button-720px.jpg)<br>

## 2D slate interaction
A 2D slate is a holographic container hosting 2D app contents, such as web browser. The design concept for interacting with a 2D slate via direct manipulation is to leverage the mental model of interacting with a physical touch screen.<br> <br>
For interacting with the slate contact:<br> 
* Users use an index finger to press a hyperlink or a button. 
* Users use an index finger to scroll a slate content up and down. 
* Users use two index fingers to zoom in and out the slate content according to relative motion of fingers. 
![2D slate image](images/2D-Slate-Interaction-720px.jpg)<br>

<br>For manipulating the 2D slate itself:<br>
* Users can approach their hands toward corners and edges to reveal the closest manipulation affordances. 
* By grabbing the manipulation affordances, users can perform uniform scaling through the corner affordnaces and reflow via the edge affordances. 
* Grabbing the holobar at the top of the 2D slate can users move the whole slate.<br><br>

![Image of slate manipulation](images/Manipulate-2d-slate-720px.jpg)


## 3D object manipulation
In HoloLens 2, users are enabled to use their hands to direct manipulate 3D hologramphic objects by applying a bounding box to each 3D object. The bounding box provides better depth perception through its proximity shader. With the bounding box, there are two design approaches for 3D object manipulation:      
### Affordance based manipulation:
It is a way for users to manipulate the 3D object through bounding box and the manipulation affordances around it. As soon as a user's hand is close to a 3D object, the bounding box and the nearest affordance are revealed. Users can grab the bounding box to move the whole object, the edge affordances to rotate and the coner affordances to scale uniformly.<br>

![3D object manipulation image](images/3D-Object-Manipulation-720px.jpg)<br>

### Non-affordance based manipulation:
In this mechanisom, no affordance is attached to the bounding box. Users can only reveal the bounding box, then directly interact with it. If the bounding box is grabbed with one hand, the translation and rotation of the object are associated to motion and orientation of the hand. When the object is grabbed with two hands, users can translate, scale and rotate it according to relative motions of two hands.<br><br> 

<br><br>
For manipulation requires precision, we recommend afforance based manipulation, providing high level of granularity. For flexible manipulation, non-affordance manipulation will be a good choice, offering users instant and playful experiences.


## Instinctual gestures
Unlike HoloLens (1st gen), teaching users a couple predefined gestures, such as Bloom and Air Tap, in HoloLens 2, we don't ask users to memorize any symbolic gesture. All gestures that users need for interacting with holograms and contents are instinctual. The way to achieve instinctual gesture is to guide users to perform gestures through the design of UI affordances. For example, if we encourage users to grab an object or a control point with two finger pinch, the object or the control point should be small. If we would like users to perform five finger grab, the object or the control point should be relatively big. Similar to buttons, a tiny button would limit users to press it with a single finger, while a huge button would encourage users to press it with their palms.
![](images/Instinctual-Gestures-720px.jpg)<br>

## Symmetric design between hands and 6 DoF controllers
You may have noticed that there are now interaction parallels we can draw between hands in AR and motion controllers in VR. Both inputs can be used to trigger direct manipulations in their respective environments. In HoloLens 2, grabbing and dragging with hands at a close distance works much in the same way as the grab button does on the motion controllers in WMR. This provides your users with interaction familiarity between the two platforms and may prove useful should you ever decide to port your app from one to the other.

## Optimizing with eye tracking
Direct manipulation can feel magical if it works as intended, but can also quickly become frustrating if you can’t move your hand anywhere anymore without unintentionally triggering a hologram.
Eye tracking can potentially help in better identifying what the user’s intent is. 

* **When**: 
Reduce falsely triggering a manipulation response. Eye tracking allows for better understanding what a user is currently engaged with. 
For example, imagine you are reading through a holographic (instructional) text when reaching over to grab you real-world work tool.
By doing so, you accidently move your hand across some interactive holographic buttons that you hadn't even noticed before (maybe it even was outside of the user's Field-of-View).
Long story short: If the user hasn't looked at a hologram for a while, yet a touch or grasp event has been detected for it, it is likely that the user wasn't actually intending to interact with that hologram. 

* **Which one**: 
Aside from addressing false positive activations, another example includes better identifying which holograms to grab or poke as the precise intersection point may not be clear from your perspective especially if several holograms are positioned close to each other. 
While eye tracking on HoloLens 2 has a certain limitation on how accurately it can determine you eye gaze, this can still be very helpful for near interactions due to depth disparity when interacting with hand input. 
This means that it is sometimes difficult to determine whether your hand is behind or in front of a hologram to precisely grab a manipulation widget for example.

 * **Where to**: 
Use information about what a user is looking at with quick throwing gestures. 
Grab a hologram and roughly toss it toward your intended destination. 
While this may sometimes work just fine, quickly performing hand gestures may result in highly inaccurate destinations.
This is where eye tracking could help out to lean the hand throwing vector back to your intended position.

## See also
* [Gaze and commit](gaze-and-commit.md)
* [Point and commit](point-and-commit.md)
* [Interaction fundamentals](interaction-fundamentals.md)


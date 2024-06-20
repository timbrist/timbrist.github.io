# Unreal Engine 5: 

Thanks for Crimson from his video [YOU'RE LEARNING UNREAL ENGINE 5 WRONG](https://www.youtube.com/watch?v=bFR6BfySY7A). The idea of learning unreal engine is to treat it as different subjects as school curriculum. 

<!-- Please come back and refine the idea here-->
Here is the subject you need to know before creating a game: 

## Asset Creation
independent game developers often download and use pre-made art assets to build their games. 
- 3D Modeling: 
using 3D-modeling software like: Blender, Maya, or 3ds Max, UE5 Plugin(beta) to create model of characters, environments, and so on.
- Texturing and Materials: 
Develop textures and materials using tools like Substance Painter or Photoshop, and import them into UE5.
- Animation
Animate characters and objects, using UE5's built-in animation tools or importing animations from external software.
- Sounds:
Not in my plan.

## Programing
- Blueprint:  
prototype and implement gameplay mechanics
- C++ programing: 
For more complex functionalities
- AI and Behavior:
Develop AI behaviors using Behavior Trees and other AI tools available in UE5.

## Hello World 

### Turn on/off a flashlight
First of all, I think It is better to start with a tiny project(demo). This youtube tutorial is very good because it only has half hour. The Author Gorka Games show you how to step by step to build a small function turn on/off a flashlight in Third Person Game. 
[Blueprint For Beginners in Unreal Engine 5 | 2023 - Learn in 30 Mins!](https://www.youtube.com/watch?v=tCJ3174CssY).


#### Summary
1. [Open the Blueprint script]  
Content Drawer -> Content -> ThirdPerson -> Blueprints -> BP_ThirdPersonCharacter(Double click)
2. [Add a cube to pretend it is a flash light and make it move as character move]  Components->Add->Cube->Details->Sockets-> Parent Socket -> (select) hand_r
3. [Add a light to the flashlight]  
Components->Add->SpotLight-> (Drag under the Cube) 
4. [When press the Key F, turn on/off the flash light]  
My Blueprint ->  GRAPHS -> Event Graph -> (program)

The 2. step is difficult to texualize. and this is also what I hate about blueprint. It is difficult to make note. 

![flashlight](https://timbrist.github.io/gamedev/turnflashlight.png)


### Compare Blueprint to real programming language
Secondly, I like to link new concepts to the things I already know.  The question is how to use if-else statement, loop, and function in blueprint?  How to print everything? 
Cason Quisenberry in his video [Need to Know Nodes in Unreal 5 Blueprints](https://www.youtube.com/watch?v=4kezN9gr_ms) present a good detail of the blueprint.

#### Summary

- if-else

- loop

- function





Add a cube and move it move with blueprint and c++.
[Unreal Engine Official Blueprints - Tutorials](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-tutorials-in-unreal-engine)
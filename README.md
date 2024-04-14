# Hyperengine Quick Start Guide 


## Table of Contents

1. [Introduction](#intro)
2. [How to Install Hyperengine](#install)
3. [How to Set Up a Non-Euclidean World](#world)
   - [Background](#w1)
   - [Instructions](#w2)
4. [How to Add and Modify Objects](objects)
   - [Background](#o1)
   - [Instructions](#02)
5. [Conclusion](#con)



## Introduction <a name="intro"></a>

*Note:* For Francesca King, I wrote the assignment in markdown because it is the conventional way to do guides in Computer Science. Markdown has it's limits when it comes to formating, I had little control over alignment and horizontal proximity.

*Note:* For Francesca King, I wrote the assignment expecting experienced developers to read it. I use relevant terminology such as parent-child and rendering. Futhermore, I also describe code, that is why you might see strange words such as randomFloor or HM.SetTileType(). They are refering to specific pieces of code. 

This is a quick start guide with examples on how to get started with the basic elements of game design with Hyperengine. It is not an in-depth tutorial that will be covering all the tools of Hyperengine. Hopefully, it will give you enough to start building your non-euclidean game and dive deeper into Hyperengine itself.

The sections 3 and 4 are divided into subsections, a background and guide. The background provides information to help understand what Hyperengine is doing and the reseaons behind Hyperengine's strange design choices. The instructions provides a set of instructions to accomplish the main task of the section. The background is not neccessary to start the instructions. The background is included to help get interested developers into the mindset of Hyperengine.



## How to Install Hyperengine <a name="install"></a>


*Note:* Hyperengine is dependent on the Unity Input System package. This can be installed to a Unity project through the Unity Registry.

The first step to using Hyperengine is having it "installed" to Unity. Hyperengine is not an official unity package nor is it a community published package. You will not be able to search for it in the Asset Store or the Unity Registry to download it. You will have to visit it's GitHub page (https://github.com/HackerPoet/HyperEngine). You can clone your own local repository on to your machine using your preferred method. If you do not want to create a local repository, you can also download the zip file from the code tab. 

After getting the Hyperengine file on your computer, the next step is to create a unity project and then close it. 

![](https://github.com/NikitaGallegos/HyperengineQS/blob/main/Install1.gif =100x20)

Finally, take the Hyperengine file and drop it directly into the assets folder of your unity project using your file manager. Because of the file structure of Hyperengine, Unity will recognize it as a package and treat it as such. Thus Hyperengine is "installed" into Unity. 

*Note:* Similarly to any other package, if you want to use another package inside the Hyperengine file itself you will have to modify the assembly file to include that packages name. 



## How to Set Up a Non-Euclidean World <a name="world"></a>


### Background: The Debug World Example <a name="w1"></a>
Hyperengine itself contains a useful example that is a great starting point for building a non-euclidean space. The example can be found in the debug folder of Hyperengine (Filepath: Assets/Hyperengine/Debug). In the Debug folder is an assortment of files needed to generate the non-euclidean world. The most important files to know are DebugWorld, DebugBuilder, RandomFloor. 

DebugWorld is the Unity scene, clicking on it will load it into the Unity editor. With Hyperengine, you can create, destroy, and modify objects in the editor like usual. However, what is seen in the editor is not guaranteed to remain the same in the actual game. The editor has a euclidean perspective and what it sees may not be what is happening in the non-euclidean space. 

DebugBuilder is the script used to generate the non-euclidean spaces by rendering special floor tiles. It is the child object of the WorldBuilder class. DebugBuilder accomplishes this by generating tiles using the RandomFloor script. Each of these generated tiles are linked to each other according to the HM.SetTileType() function located in the MaxExpansion function as seen below. Hyperengine defines the space by the number of allowed tile connections, thus the argument in HM.SetTileType() will determine which space DebugBuilder creates. 

#### HM.SetTileType() Inputs
| Arguement     | Space         |
| ------------- |-------------  |
| 3             | Spherical     |
| 4             | Normal        |
| 5-6           | Hyperbolic    |

#### DebugBuilder Code
    public class DebugBuilder : WorldBuilder
    {
            public static int RADIUS = 6;
            public GameObject debug_tile;
        
            public override int MaxExpansion()
            {
                HM.SetTileType(3);
                return RADIUS;
            }
        
            public override GameObject GetTile(string coord)
            {
                return Instantiate(debug_tile);
            }
    }

*Note:* Do not be misled by the radius variable, it is not the radius of the non-euclidean space, adjusting it will not make the space smaller or larger. It is a variable that aids in generating the player's collision box.

RandomFloor is a script that generates the number of planes requested. Each plane is a random color and is associated with a coordinate stored as strings in a text file. The use of strings to define the position rather than using non-euclidean coordinates is that non-euclidean coordinates are not intuitive to read and can change depending on certain factors. Instead, the tiles coordinates are defined by their orientation from the origin tile. For example, the string "URRD" means that tile is located one tile down, two tiles to the right, and one tile down from the origin tile. 

When building a non-euclidean world in Hyperengine it is best to refer to these three files and understand how they interact. Every working non-euclidean space in Hyperengine uses its own version of these three files. It is good to start by copying these files and using modified versions to generate different non-euclidean worlds. 


### Instructions: Creating Your Own Non-Euclidean World <a name="w2"></a>
**CAUTION:** When activating play mode in the unity editor your computer antivirus may flag it as malware and quarantine your Unity files. The quarantine while unity is running a game may cause corruption, damaging unity data or software. 

With Hyperengine installed it is time to build your first non-euclidean world. Begin by copying the DebugWorld, DebugBuilder, and RandomFloor from the Hyperengine debug file (Filepath: Assets/Hyperengine/Debug). Place the copies in their own folder in Hyperengine. Rename the copies to whatever you wish. Make sure to change the class name in the code itself as well. Create a plane object to be the floor tiles for the renamed DebugBuilder. 

*Note:* Copying over the debugTile object instead of creating a plane also works

Load in the scene by clicking your renamed version of DebugWorld. Set a space for builder file to create by inputting the argument for HM.SetTileType(). Input 3 for spherical space. Input 4 for nornal euclidean space, Input 5-6 to get Hyperbolic space. 

#### HM.SetTileType() Inputs
| Arguement     | Space         |
| ------------- |-------------  |
| 3             | Spherical     |
| 4             | Normal        |
| 5-6           | Hyperbolic    |

Then modify the RandomFloor function, using the typical Unity texture and color class to create the tiles that will be the floor of the space. Click the playtest button and go to the game tab. The virtual world should clearly be rendering the space set in the builder file. It is ready to be populated by hyper objects to form a non-euclidean game.

**Note:** The difference between input 5 and input 6 is that input six will have more tiles connected to eachother. It will make distances in the hyperbolic space much more inconsistent.

*Note:* You can also copy over the DebugTile scipt with the letter objects. This will add labels to tiles. It is quite helpful to orient yourself while debugging.

*Note:* Spherical Space is a bounded space. Hyperengine statically defines the bounds of spherical, meaning the bounds cannot be adjusted. Some developers may find themselves without enough space to build a satisfying game. Instead of making the world larger, make the player smaller. 



## How to Add and Modift Objects <a name="objects"></a>


### Background: Objects and Hyperobjects in Hyperengine <a name="o1"></a>

Unity objects do not work entirely in Hyperengine. Hyperengine prevents the rendering of normal objects. It only chooses to render objects that are children of hyper objects. The hyper object parent-child relationship exists because, in Hyperengine, the position of an object is determined the gyrovector and the local vector. Gyrovectors are a niche vector space that preserve rotational data in non-euclidean spaces. They are necessary for Hyperengine to generate anything non-euclidean. The local vector sets the objects position relative to its parent's gyrovector. The local vector works because the difference between close objects can be described by a vector and a gyrovector. The local vector is employed to reduce computation costs because gyrovector operations are much more computationally expensive than vector operations. Therefore when objects are close (such as those with the same parent), it is more economical to use vectors instead of gyrovectors. 

Furthermore, Hyperengine replaces the physics engine of Unity entirely. Unity's physic engine is not compatible with non-euclidean spaces. Currently, the Hyperengine physics only provide colliders called warp colliders. Warp colliders are scripts that constantly calculate the surface of the frequently warping hyper objects. Warp colliders themselves do not detect collisions and send messages. Hyperengine’s player code is constantly checking to see if it is contacting the surface calculated by warp colliders. Unlike unity the colliders do not detect collisions themselves. 


### Instructions: Adding and Modify Objects <a name="o2"></a>

**CAUTION:** Avoid using the hyper canvas object in a 2022 version of Unity or newer. Unity has overhauled the canvas renderer causing crashing and corruption when Hyperengine attempts to take them. 

It is now time to fill your non-euclidean space with Unity objects and build your game. Begin with creating an empty object by right clicking the scene and navigating to create empty. The empty project will be the parent of the objects that the scene will render. Go to the empty object and add the hyper object component. 

The next step is to create the child object that will exist in the game. Right click the parent you made and navigate down to the object you want to create. Now the object is the child. Left click on the parent to see it's children. Click on the child object. Add the warp collider and set texture components to the object. Lastly, delete the non warp collider component.

The child object will now render in the non-euclidean space. If you want to change the object's postion by a small amount, edit its transform component. Otherwise, to largely change the position of a hyper object navigate to it's parent. From there go to the hyper object component. In the hyper object component is the gyr coordinate, change the gry coordinate to move the position of the parent and it's children. 

*Note:* Do not trust the scene editor display. Where objects appear in the editor are hardly where they actually are in the non-euclidean space. 

*Note:* Hyper objects are landlocked to the current tile they reside on unless the gyrovector of the parent is changed. 

The final step is to add warp colliders. Find the child object and navigate to the warp collider component. In the warp collider you will see six tabs with numbers next to them.

- Bounding Boxes
- Bounding Spheres
- Bounding Triangles
- Bounding Planes
- Bounding Meshes

The tabs describe a collider's shape and the number is how many colliders are already applied. Click on the tab you believe to be necessary. Click the "+" button. A box will appear in the warp collider component, this box has inputs for the colliders position and size. Furthermore, the collider will appear in the scene as a green outline. Change the inputs the box to manipulate the collider to your liking. To remove any colliders simply click "-" under it's associated box.

*Note:* The “remove triang” checkbox indicates to Hyperengine to ignore colliders in direct contact with the floor because the player will never be able to touch them.

*Note:* Constantly redoing all these steps for each object added to the game can be overwhelming. Create and use prefabs out of the existing hyper objects to preset these steps.  

## Conclusion <a name="con"></a>

Hyperengine is much deeper then what can be provided in a brief quick start guide. However, you should now have enough tools to get started and learn at a fair pace. It should also be noted as time passes and Unity is updated, Hyperengine will become more depricated. It is best to start working with Hyperengine sooner than later.

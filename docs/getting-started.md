# Getting Started  

In this section, we'll walk you through setting up the **Dot Matrix Shader**.  

## 1. Project Settings  

Before using this asset, you need to enable **Custom Depth** in your project settings. To do this, go to:  
`Edit -> Project Settings`, then search for **"Custom Depth"**. Locate the option `Custom Depth-Stencil Pass` and set it to `Enabled with Stencil`.

![alt text](<images/Screenshot 2025-04-01 175633.png>)

## 2. Post-Process Volume  

After enabling **Custom Depth**, the next step is to add a `Post Process Volume` to your scene. Alternatively, you can use an actor with a `Post-Process Component`.  

Make sure to check the `Infinite Extent` option to apply the effect globally.

![alt text](<images/Screenshot 2025-04-01 180055.png>)

![alt text](<images/Screenshot 2025-04-01 180419.png>)

## 3. Adding Materials  

Now, select your ``Post Process Component`` and search for **"Materials"** in the ``Details`` panel. You'll see an empty list—click the **plus icon** and choose your preferred **Dot Matrix** material.

![text](<images/Screenshot 2025-04-01 180618.png>)
![alt text](<images/Screenshot 2025-04-01 180710.png>)

!!! note
    Do not use **surface** variants for the post process component.

## 4. Assigning Custom Depth

If everything is set up correctly, you might notice a solid color or no visible changes in your viewport.  

![alt text](<images/Screenshot 2025-04-01 181121.png>)

This is expected—since no specific mesh has been targeted yet, only the background color is rendered.  

![alt text](<images/Screenshot 2025-04-01 181552.png>)

To see the effect in action, add a **sphere** (or any mesh) to the scene and enable **Custom Depth**. Select the mesh, in the **Details** panel search for **"Custom Depth"** and enable `Render CustomDepth Pass`. Also set the `CustomDepth Stencil Value` to **77**.  Now, the **Dot Matrix** effect should appear on the selected mesh! 🎉

![alt text](<images/Screenshot 2025-04-01 181606.png>)


## 5. Using Surface Variants

Some variants of the shader can be applied directly to any mesh. Simply **drag and drop** the materials denoted with "Surface" onto the desired surface, and it will work right away!

![alt text](<images/Screenshot 2025-04-01 182811.png>)

---

## Adding Your Own Patterns

There are two variants of this shader: **Regular** and **Flipbook**. Assigning custom motifs vary for each, so let's see how its done.

### Regular Variant

Regular variant of the Dot Matrix shader uses individual textures to act as motifs. This means adding more motifs will require duplicating a few simple nodes in the material graph. This process may seem tedious but it comes with the benifit of a more **random** grid.

1. Open the parent material of choice, here I'm using the **M_DotMatrixR**.

    ![alt text](<images/Screenshot 2025-04-01 183802.png>)

2. In the texture samples section of the material nodes, select any of the ``Texture Sample`` node and change its texture in the details panel.

    ![alt text](<images/Screenshot 2025-04-01 184008.png>)

3. To **add** more patterns, simply duplicate the ``Texture Sample`` node and assign any excess textures.

    ![alt text](<images/Screenshot 2025-04-01 184457.png>)

4. Next make sure the new ``Texture Sample`` node follows the same lerp format. 

    ![alt text](<images/Screenshot 2025-04-01 184912.png>)

5. For the ``Alpha`` we'll duplicate a ``Step`` node in the **Step Node** section. Make sure the **Y** value corresponds to the number of the texture. So the fourth texture will have a value of **4**.

    !!! CORRECTION
        The floor input should go to **X** and the step number to **Y**.

    ![alt text](<images/Screenshot 2025-04-01 184949.png>)

6. Connect this returning value to the alpha of the ``Lerp``. Finally connect the output of the ``Lerp`` node to the ``ManualPatterns`` reroute.
    
    ![alt text](<images/Screenshot 2025-04-01 185025.png>)

To see additional patterns in effect, make sure to increase or decrease the ``NumberOfTextures`` parameter in the material/material instances.

![alt text](<images/Screenshot 2025-04-01 190316.png>)


### Flipbook Variant

Flipbook variant of the Dot Matrix shader uses pre-defined altases or flipbooks for its grid patterns. This is far easier to modify but comes at the cost of less appealing repititions.

1. Open any flipbook material and simply assign your own texture to the ``FlipbookPattern`` parameter. 

    ![alt text](<images/Screenshot 2025-04-02 101347_1.PNG>)

2. Set the appropriate number of rows and columns as seen in your custom flipbook.

    ![alt text](<images/Screenshot 2025-04-02 101509.png>)

## Using Scene Color as Background 

![alt text](<images/Screenshot 2025-03-24 153145.png>)

If you're looking to create the above effect, there are a couple things to take into consideration. 

1. This effect can be achieved using the **"BG"** variants of the dot matrix shader.

    ![alt text](<images/Screenshot 2025-04-02 102306.png>)

2. However once you apply it to the post-process volume, you'll notice the surface material of the mesh leak through the motifs.

    ![alt text](<images/Screenshot 2025-04-02 102447.png>)

3. To fix, this simply select the mesh in question and in the ``Material`` parameter, assign the **M_TransparentBase** that comes with the pack.

    ![alt text](<images/Screenshot 2025-04-02 102708.png>)
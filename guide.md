# Topology Optimization Guide
This guide is meant to be a step-by-step(ish) description of the general process that was used to design the front and rear wing mounts on fb25. 
My goal while writing this is to make it as generalized as possible so it can be applied to design applications outside of just aero wing mounts. 
The workflow is definitely not optimized either, so I encourage people to look for ways to potentially improve it. 

## Step One: Defining your support points
Before you begin designing your part, you first need to determine what the part is actually going to attach to. Using the rear wing as an example, we clearly need to design the mount to 
span from the two attachment points on the wing to the two brackets on the roll hoop.

<figure>
  <img src="figure1.png" alt="image" width="500">
  <figcaption>First identify your mounting points.</figcaption>
</figure><br><br>

## Step Two: Defining Your Maximum Material Area
The ansys structural optimization method that we will be using (Density Based Topology Optimization) works by determining the lowest possible material density in each cell of the part we are optimizing, to meet a certain load case and set of criteria (in very oversimplified terms). This means we will first need to give the software a large solid part to work with, as this optimization method can optimize material that alread exists in the part. In the case of most 2d plates like this, the best way to define the starting material is to create the outline of what you want your final plate to look like. I encourage you to briefely skim through the ansys documentation [here](https://ansyshelp.ansys.com/public/account/secured?returnurl=/Views/Secured/corp/v251/en/mech_struct_opt/ds_topo_opt_limitations.html) to learn a bit more about how this methodology works.<br><br>

<figure> 
  <img src="example blank wing plate drawn.png" alt="image" width="500">
  <figcaption>Example of a plate outline (watch out interferences and the height limit!)</figcaption>
</figure><br><br>
<figure>
  <img src="rw mount outline part.png" alt="image" width="500"
    <figcaption>Create your outline part in solidworks.</figcaption>
</figure><br><br>

Note: It is important in this step to keep in mind the design rules and watch out for interferences. Make sure your plate doesn't interfere with any rear wing elements and isn't so tall as to become illigal via the rules. See [T 7.7](https://www.bing.com/ck/a?!&&p=31d652aacf2c253635ddbf2cef5e78a2e902b9649fdfdee165f7b8406cfcdb3bJmltdHM9MTc3MTM3MjgwMA&ptn=3&ver=2&hsh=4&fclid=10275a2f-76f2-6c6c-0dac-4d2b77e96d8c&psq=fsae+2026+ev+rulebook&u=a1aHR0cHM6Ly93d3cuZnNhZW9ubGluZS5jb20vY2Rzd2ViL2dlbi9Eb3dubG9hZERvY3VtZW50LmFzcHg_RG9jdW1lbnRJRD0yNzhmZDRkNy1hYTI3LTRlMzMtYmM0YS0wOTAxNDhlNjYyYTA) on page 71 of the rulebook for aerodynamic devices, specifically the height limit. <br><br>

## Step Three: Initial Static Structural Simulation
In order to run the topology optimization program, we first need to run a static structural simulation on our part using the expected load cases.

### First, open Ansys Workbench and create a static structural simulation

<https://github.com/user-attachments/assets/8acda7a6-9830-4eab-b93f-c006cb2a2ac7> 
<!-- Open Static Structural -->


### Define the material of the part
Here we come to a bit of an issue. The parts that we are designing to go on the car will be made of aluminum 7075T6, however when I designed these parts last year, I vaguely remember being advised to run the optimization program using structural steel instead of aluminum 7075. I believe it was because the optimization method we are using doesn't work very well with aluminum, but I cannot confirm this and haven't actually compared the results using each material. It may be true, or it may be some urban legend passed down through generations - if someone wants to run the same simulation in parallel with aluminum and steel to test that out, that would be really cool! For the guide i'm just going to say to use steel because I did it last year and it worked well enough. 

<https://github.com/user-attachments/assets/ce0d8cbe-8616-4e0c-a807-593c76817163> 
<!-- Assign Material -->


Note: You may come to find that the material is already assigned as structural steel by default, in which case you don't need to do anything!

### Add loads and constraints
For the sake of this guide, i'm just adding generalized forces. When you do this project for real, you should be provided with loads by Sam and Viktor. 

<https://github.com/user-attachments/assets/d20604d3-de06-415a-a8b5-dc6ea270ce3c> 
<!-- Apply bearing load -->

<https://github.com/user-attachments/assets/2394b12c-9fed-406f-825e-23ae953c89e7> 
<!-- Apply Cylindrical Support -->


Note: Make sure to apply a different bearing load for each hole, or the force you specify will be distributed over the multiple holes selected
Note 2: Hold Ctrl to select multiple faces at a time

### Create your mesh
The final step before you are finished with the static structural setup is to create your mesh. To do so, we will need to define two parameters - meshing methodology and mesh size. 

To select a meshing methodology, right click the "mesh" section on the left and insert method. I usually default to a tetrehedral mesh, as it seems to deal with curved surfaces and holes the best, however I once again would be happily be proven wrong if someone wants to look into optimal meshing methods for this type of FEA. 

<https://github.com/user-attachments/assets/9c53bc75-6b42-4e3d-a745-f45413030bcf> 
<!-- Meshing Method Selection -->


To select a mesh cell size, again right click the "mesh" section and insert sizing. A good rule of thumb is to make sure that your mesh is fine enough that areas that may see significantly different stresses near each other (eg. a thin beam in bending or corners with small radii) have enough cells that those stresses won't be averaged out across one big cell and potentially skew your results. The downside to a finer mesh is longer simulation times, as the computer has to solve for more cell conditions. A 5mm mesh should be good enough for almost all of our useceses and solves pretty quickly on small parts.

<https://github.com/user-attachments/assets/6cfab391-1a15-42cd-8dd7-52b6b4f2d8ff> 
<!-- Meshing Size Selection -->

### Run the static structural simulation
When you are finished with the last steps, the screen on the right should look like the image below, with green check marks next to each setup category. If any category (other than results) doesn't have a green check mark, you can right click it and select "update."
<figure>
  <img src="Static Structural Ready to Run.png" alt="image" width="800">
  <figcaption>Once your screen looks like this, hit "solve" to run the simulation.</figcaption>
</figure><br><br>

Once the simulation has run, go back into workbench and make sure that all the categories of your static structural simulation are resolved (with green checkmarks). 
<figure>
  <img src="Static Structural Finished.png" alt="image" width="800">
</figure>

## Step Four: Run Structural Optimization (aka topology optimization) Simulation
At this point, we are now finished with our static structural simulation and there is enough upstream data for the structural optimization sim to use for its topop. To set up the structural optimization sim, we will follow similar steps to the ones we used for our static sim. 

### Connect the structural optimization block to the static structural simulation
To configure the optimization sim to use the data from the first simulation, we need to go into ansys workbench and drag a structural optimization sim into our project. Make sure to drag it directly onto the "solution" box of the static structural sim, or else the rest of the sections will not connect properly. 

<https://github.com/user-attachments/assets/cd4e9391-a0ac-40c2-9701-9c2f3573ff19>
<!-- Insert structural Optimization -->


### Run the Topology Optimization Sim
To setup the topop sim, click setup on the structural optimization block and select edit, just as we did for the original static structural. Most of the default settings will be good for us, however we need to set up exclusion regions that we don't want material to be removed from. To do this, go into the optimization region menue and change the exclusion region to be defined by geometry selection. We want to select the entire outline of our part, as well as the inside edges of any holes. 

<figure>
  <img src="Exclusion region via geometry selection.png" alt="image" width="500">
  <figcaption>Select the exclusion region using geometry selection.</figcaption>
</figure><br><br>

<figure>
  <img src="properly selected exclusion region.png" alt="image" width="500">
  <figcaption>This is what your selection should look like.</figcaption>
</figure><br><br>

At this point, you can run your first iteration of the optimization.

### Analyze and Tune Results 
When your simulation finishes running, you might be left with a result that looks something like this.
<figure>
  <img src="Garbage Results.png" alt="image" width="500">
  <figcaption>These results are garbage and lowkey tell us nothing useful. However, we can fix that!</figcaption>
</figure><br><br>

Through some experimentation I figured out that by adjusting the parameter "percent [mass] to retain," the results could be tuned to be much more useful for our design. I found the best results around 20%, however I would encourage you to experiment with values around that for your own mount designs. You can find this parameter in the "response constraint" menue. 

<figure>
  <img src="Good Results.png" alt="image" width="500">
  <figcaption>These results tell us much more efficiently how to reduce the material of the part while retaining strength</figcaption>
</figure><br><br>

There are other ways that we can fine tune our results. The optimization method we used gives results based on material density, so we can effectively increase/decrease the amount of material that is removed by changing the minimum density threshold. To do this, select topology density and change the "retained threshold value" between 0.01 (most material) and 0.99 (least material). You can ignore the other values such as mass and volume, because we only use the results of this simulation as a guide for the actual part design. 

<figure>
  <img src="Retained Threshold.png" alt="image" width="500">
</figure><br><br>


## Step Five: Model Your Part
Once you are happy with the results of your simulation results, you need to use them to model your part. The best way that I have found is to take a screenshot of the simulation results and insert them as an image to solidworks. You can then trace over the approximate shapes that you want and be left with an approximately solid part. 

<figure>
  <img src="Screenshot Approximation.png" alt="image" width="500">
  <figcaption>This is a reference from the design of the fb25 rear wing mounts. As you can see, the initial topology was used as an itial approximation before iterative changes were made to improve the strength and efficiency of the part. Topop is a good starting point, but huge gains can be made from good design.</figcaption>
</figure><br><br>

At this point, the last step is to simply iterate on your plate designs until you find something you are happy with or start seeing diminishing returns. The fb25 plates went through many versions of relatively different pocketing approaches using the FEA outline, before eventually settling into the final design. It is essential to validate your design work by running normal static structural simulations on your lightened parts versions, and comparing the performance between each. 

<figure>
  <img src="version control.png" alt="image" width="500">
  <figcaption>*Final note- do NOT use onshape for this, the team standard is solidworks!!!</figcaption>
</figure><br><br>

Good luck!


<!-- "S:\CAR\SP\Formula\Car 25 Vehicle (EV)\Car 25 Vehicle CAD\Aerodynamics\RW\Swan Neck\pald testing\Iteration 1\swan-neck-fea.wbpj" 
-->

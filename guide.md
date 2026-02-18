# Topology Optimization Guide
This guide is meant to be a step-by-step(ish) description of the general process that was used to design the front and rear wing mounts on fb25. 
My goal while writing this is to make it as generalized as possible so it can be applied to design applications outside of just aero wing mounts. 
The workflow is definitely not optimized either, so I encourage people to look for ways to potentially improve it. 

## Step One: Defining your support points
Before you begin designing your part, you first need to determine what the part is actually going to attach to. Using the rear wing as an example, we clearly need to design the mount to 
span from the two attachment points on the wing to the two brackets on the roll hoop.

(insert formatted image of the rear wing with pauls balls mounts circled in green and rollhoop mounts in red)

## Step Two: Defining Your Maximum Material Area
The ansys structural optimization method that we will be using (Density Based Topology Optimization) works by determining the lowest possible material density in each cell of the part we are optimizing, to meet a certain load case and set of criteria (in very oversimplified terms). This means we will first need to give the software a large solid part to work with, as this optimization method can optimize material that alread exists in the part. In the case of most 2d plates like this, the best way to define the starting material is to create the outline of what you want your final plate to look like. I encourage you to briefely skim through the ansys documentation [here](https://ansyshelp.ansys.com/public/account/secured?returnurl=/Views/Secured/corp/v251/en/mech_struct_opt/ds_topo_opt_limitations.html) to learn a bit more about how this methodology works.<br><br>

<figure>
  <img src="figure1.png" alt="image" width="500">
  <figcaption>First identify your mounting points</figcaption>
</figure><br><br>
<figure> 
  <img src="example blank wing plate drawn.png" alt="image" width="500">
  <figcaption>Example of a plate outline (watch out interferences and the height limit!)</figcaption>
</figure><br><br>
<figure>
  <img src="rw mount outline part.png" alt="image" width="500"
    <figcaption>Create your outline part in solidworks</figcaption>
</figure><br><br>

Note: It is important in this step to keep in mind the design rules and watch out for interferences. Make sure your plate doesn't interfere with any rear wing elements and isn't so tall as to become illigal via the rules. See [T 7.7](https://www.bing.com/ck/a?!&&p=31d652aacf2c253635ddbf2cef5e78a2e902b9649fdfdee165f7b8406cfcdb3bJmltdHM9MTc3MTM3MjgwMA&ptn=3&ver=2&hsh=4&fclid=10275a2f-76f2-6c6c-0dac-4d2b77e96d8c&psq=fsae+2026+ev+rulebook&u=a1aHR0cHM6Ly93d3cuZnNhZW9ubGluZS5jb20vY2Rzd2ViL2dlbi9Eb3dubG9hZERvY3VtZW50LmFzcHg_RG9jdW1lbnRJRD0yNzhmZDRkNy1hYTI3LTRlMzMtYmM0YS0wOTAxNDhlNjYyYTA) on page 71 of the rulebook for aerodynamic devices, specifically the height limit. <br><br>

## Step Three: Initial Static Structural Simulation




<!-- "S:\CAR\SP\Formula\Car 25 Vehicle (EV)\Car 25 Vehicle CAD\Aerodynamics\RW\Swan Neck\pald testing\Iteration 1\swan-neck-fea.wbpj" 
-->

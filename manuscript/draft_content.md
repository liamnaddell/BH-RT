1. Abstract
2. Introduction
     * Background on the kind of black hole
     * how the differential equation under study works
3. Implementation
     * how we can model the differential equation in GSL
     * how the raytracer works.
     * Domain decomposition with MPI (Potentially some images could be larger than system ram, talk about how this is handled)
     * Parallel processing with OpenMP
4. Results
     * Some rendered images
     * Scaling results after scaling image dimensions, potentially some other scaling analysis
       which I previously did for my presentation.
5. Related and Future work
     * This technique can be used to raytrace any kind of image under light distortion
     * Kerr metric (This was done by some other people before)
6. Summary

1. Abstract

The path a light ray takes in orbit of a simple black hole can be modeled by the partial differential equation
$$u^\prime\prime - u = 3Mu^2$$, which relates the euclidean distance and azimuthal angle between the light ray
and the black hole center, to the mass of the black hole. Using GSL and standard raytracing methods, we can trace
the path of a light ray through 3D space, and determine the interactions that light ray may develop in a 3D scene.
This comes at the cost of significant computational complexity, which is handled by decomposing the problem with
MPI, and rendering distinct rays in parallel.

2. Introduction

The Schwartzschield metric can model black holes which, among other properties, are non-spinning and not charged, 
meaning their deformation of the trajectories of light rays do not depend on the angle of approach. This paper
(https://phys.au.dk/~fedorov/old/gtr/2018/note9.pdf) Shows how we can derive the equation $$ u'' - u = 3Mu^2 $$, which
relates the trajectory of a light ray to the mass of a black hole, from the Schwartzschield metric. For this equation,
"u" represents 1/r, the euclidean distance of a point on the light ray to the black hole center. "u" is differentiated
with respect to $$ \phi $$, the azimuthal angle between the point and the black hole origin. After providing initial
conditions, we are able to use this equation to trace the entire trajectory of the ray with high fidelity. Notably, this
equation is modeled in 2d-sphericial coordinates, meaning, if we want to use this equation to analyze light rays in 3D
space, we need to find a way to find an equivalent ray that our equation models, and use the 2D ray to find the 3D ray's
trajectory.

After being able to study the trajectory of individual rays, we wish to be able to use this insight to generate images.
The natural technique is to use a ray tracer. Ray tracers render a simulated scene by casting light rays from a
simulated camera, and computing what objects in the scene would be visable to that ray. If a particular camera would
produce an image of 1920x1080 pixels, we could build the image that camera would record by casting a light ray through
each pixel. Because ray tracing's unit of perception is how simulated light rays interact with a simulated scene, it is
a natural choice for simulations which involve the bending or distortion of light.

3. Implementation

While the fundamental idea of taking a ray tracer and modifying it so that light rays are distorted is sound, the
implementation of such an idea is fraught with difficulty. The first major challenge is that ray tracers need linear
equations to be able to compute object intersection in a scene. This means we need to find a way to describe our light
ray as a piecewise-defined set of linear equations. We found the best way to do this was to compute discrete points on
the light ray, and compute intersection using the line segments defined by said points. 

As for how to compute these segments, GSL's suite of ODE approximation tools found significant utility. GSL allows us to
define our equations as C functions, and our particular ray by a set of initial condtions. From there, we can use GSL to
compute a series of discrete points on the ray governed by a timestep of our choosing. For initial conditions, we need
to describe,in 2D spherical coordinates, an initial point on the ray and a single successor point, which was computed
using the linear direction, as we are casting rays far from the black hole's influence. As for the timestep (as it is
commonly known in the GSL documentation), we allowed program users to control the "epsilon" of the simulation, with
lower epsilon values resulting in more accurate trajectories. 

To compute the next point on a light ray `epsilon` distance away, we would compute, using the euclidean metric, the light ray's path with no distortion. In sphericial coordinates, this allows us to compute the azimuthal angle from the black hole's origin. We can feed this updated azimuthal angle into GSL, and it will return the distance of the ray from the origin of the black hole. In effect, GSL behaves as a function from azimuthal angle to distance, allowing us to compute the needed points in 2D space.  [TODO: needs diagram]

As noted earlier though, we cannot use the trajectories of rays in 2D space to render a 3D scene, meaning we need to
find a way to find an invertible relationship between 3D rays and 2D rays. The solution here was to recognise that we
could apply a combination of axis rotations and axis relabling to complete the correlation. If the "origin" of the light
ray, and the origin of the black hole are both located on the z=0 axis, as is the case for our 3D scenes, we can rotate
our ray around the z axis until the ray's direction vector has incline $$ \pi / 2 $$ with respect to the black hole
origin, meaning the ray has no spherical incline. At this point, the y component of the ray's origin and direction will
both be zero, meaning we have reduced our 3D ray to 2D. From here, we can easily convert to spherical coordinates for
use with GSL. Since this process is invertible, we have now found a way to use GSL to trace the trajectory of light rays
in 3D using an equation of two variables. [TODO: Fact check, crappy explanation, needs diagram].

4. Results

4.1. Some rendered images.

Mostly 

4.2. Scaling analysis

TODO: Need to actually run the scaling analysis



5. Discussion

5.1. Educational value.

This raytracer was originally developed for educational purposes in the context of Dr Marcelo Ponce's course CSCD71. As
a project, it combines physics simulations (both through the use of a raytracer, and through the simulation of
gravitational distortion), ODE approximation with GSL, path stenciling, image/file parsing (esp. at large scale),
parallel and distributed computing with MPI and OpenMP, domain decomposition, as well as providing a bountiful quantity
of variant implementations, while being of low-enough difficulty to be completed in a few weeks (with great difficulty).
The result of which is the ability to render pretty images. In particular, the project would provide potential students
an excellent opportunity to learn the practical use of GSL/ODE approximation in a realistic context. The impact is felt
in 3 areas:

1. Attempting to set up this particular system is theoretically easy and practically quite difficult. The initial
   conditions are not obvious. GSL asks for an initial radius, initial angle, and the derivative of the radius with
   respect to azimuthal angle. Unfortunately, it is not possible to find this derivative without an equation that models
   the trajectory of light rays, which is the equation we were trying to approximate in the first place! Solving this
   chicken-and-egg problem is a great educational experience, and forces one to consider more deeply the significance
   of the parameters given to GSL. (TODO: Debugging is hell?)
2. Using GSL for course analysis is a nonstandard application. One key detail of the project is that we need to restrict
   the number of points GSL returns, while keeping the distance between points relatively even. (TODO: future work:
   dynamic spacing based off of distortion). The way this challenge was solved was by approximating the angle between
   the black hole on a line of potential future points, as described earlier, and having the GSL driver stencil the
   equation until the desired 


credit:
marcelo (where is appropraite? Mention CSCD71?)
Ray Tracing in One Weekend

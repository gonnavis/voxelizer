# What is it?
**voxelizer** is a tool that converts 3D models in [Wavefront OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file) format to a 3D texture including transparency. The output texture corresponds to the [AABB](https://en.wikipedia.org/wiki/Minimum_bounding_box#Axis-aligned_minimum_bounding_box) of the scene. Each 3D texel of the output contains the average color of objects intersecting it and has the alpha channel proportional to the volume of intersection. Separate objects are treated individually and are simply added together to the output (overlow is clamped at 1.0).
The default object color is opaque white. You can change it by setting an appropriate material in the MTL file.
See the examples directory for, well, examples :)
# How does it work?
For each 3D texel of the output texture, it samples quasi-random locations withing that texel, checks if they are contained in the objects by shooting a ray in the positive X direction and counting the number of intersections, and averages over the samples. Sampling is done using the [R3 sequence](http://extremelearning.com.au/unreasonable-effectiveness-of-quasirandom-sequences).
Then, it performs a flood-fill on untouched transparent black pixels to prevent black color leaking when filtering the texture close to object boundaries.
# How fast is it?
It uses a simple ad-hoc implementation of an R-tree to speed up intersection tests, and uses all available cores when sampling. It takes 152 seconds to convert a Stanford bunny model with 5002 triangles into a 256x256x256 texture using 256 samples per texel on a 16-core i9-9900K 3.60GHz. Not blazing fast, but gets the job done.
# Can I use it?
Sure.
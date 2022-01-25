The spatial analysis consists of the calculations for  

* Distance to entrance
* Noise field from street

* Solar envelope
* Sky view factor
* Distance to facade

### The Distance to Entrance

The distance to entrance will be calculated with 5 different entrances. In the figure below the 5 entrances are shown with blue dots.

<center>
    ![](../img/a3/sa/entrances.png)
</center>

First the distance to each distance is calculated with the Euclidean method. This is done with the following code:

```python
substract centroids of voxels

for each centroid:
    distance_vector = []
    for each entrance:
        difference_vector = centroid - street_point
        distance = squareroot(difference[0]**2 + difference[1]**2)
    add to the distance matrix
make list of distances

distance to closest street point = distance.min()
Convert to lattice
```
This gives the following result:
<center>
    ![](../img/a3/sa/distance entrance eucl.png)
</center>
These Euclidean distances are then used to select the closest entrance for each entrance. This is actually not needed and does not speed up calculation time as the manifold distance is calculated with breadth first traversal. The following pseudocode is used to calculate the manifold distance:

```python
select closest voxels from euclidian distance
create stencil

neighbours = closest_voxels.find_neighbours(stencil)

set furthest entrances to maximum distance and closest to zero in lattices

for each centroid:
    find neighbours of last step
    validate neighbours if inside envelope
    select next steps
    save the 'walked' distance
    extract minimum distance
    if all cells == filled:
        break

Construct lattice of minimum distances
```

These calculations give the following result:
<center>
    ![](../img/a3/sa/distance to entrance.png)
</center>

###  The noise field from street 

The noise field from the street has been made by extracting the volume of three streets. The streets 'Boekhorststraat' with 60 decibel, 'Schoterbosstraat' with 50 decibel and 'Van Bokelweg' with 70 decibel. The streets are shown below:

<center>
    ![](../img/a3/sa/noise streets.png)
</center>

The streets are lines of points. The distance to each point for each voxel centre is calculated and then the resting volume is calculated by a logarithmic function. You can see the pseudocode below:

```python
extract voice_centroids and put in lattice
add noise volume to point clouds noise sources

for each noise source:
    for each centroid:
        distance_lattice = sp.spatial.distance.euclidean(centroid, noise source)
    
    noise_lattice = noise_base - 20 * np.log10(dist_lattice) - 8

    sum_noise += np.power(10, noise_lattice / 10)

# Repeat for each noise source

aggregate_noise_lattices = 10 * np.log10(sum_noise)
```

This will give the following result:

<center>
    ![](../img/a3/sa/noise field.png)
</center>

###  The Solar Envelope 

The Solar Envelope is one word for both sun light access and shadow analysis. The shadow analysis will furthermore be used in shaping of the envelope.

####  Sun Light Access 

For the sun light we choose 4 days representing 4 typical days in 4 seasons at the building's location.  

```python
sp = Sunpath(longitude=4.3571, latitude=52.0116)

hoys = []
sun_vectors = []
day_multiples = 90

for d in range(365):
    if d%day_multiples==0:
        for h in range(24):
            hoy = d*24 + h
            sun = sp.calculate_sun_from_hoy(hoy)
            sun_vector = sun.sun_vector.to_array()
            if sun_vector[2] < 0.0:
                hoys.append(hoy)
                sun_vectors.append(sun_vector)
```

Then we set the sun light to the reverse direction and shoot them from each voxel of the lattice.  

```python
sun_dirs = -np.array(converted_sun_vectors)
vox_cens = envelope_lattice.centroids

for v_cen in vox_cens:
    for s_dir in sun_dirs:
        ray_dir.append(s_dir)
        ray_src.append(v_cen)
```

Finally we count the percentage that those rays will be blocked.

```python
sun_access = 1.0 - int_count/sun_count 
```

<center>
    ![](../img/a3/sa/sunlight.png)
</center>

####  Shadowing 

The shadow casted by this building is done by a similar idea. The only thing changed was to choose the original sun light directions (instead of reversed ones), such that from those rays shooted, we can know the percentage of time how the building could possibly block sun light for the surrounding buildings.   

<center>
    ![](../img/a3/sa/sunblock.png)
</center>

###  The Sky View Factor 

The sky view factor means the percentage of open sky we can see at a specific point. In places like midtown Manhatten, the sky view factor is very low due to the large amount of sky scrapers. And in comparison, in the middle of a desert the sky view factor and be close to 1, as there is no blockage anywhere.  

This idea can be useful in tackling Urban Heat Island Effect. It is also interesting to use it for other interesting measures.  

The implementation of it is rather simple. Using the same library as above, we can create a semisphere of rays.  

<center>
    ![](../img/a3/sa/semisphere.png)
</center>

Then we compute the percentage that those rays hit the surroundings.

<center>
    ![](../img/a3/sa/skyviewfactor.png)
</center>


###  The Distance to Facade 

The distance to facade means basically the closest distance from the voxel to outside. By calculating the inner voxels, we get the lattice of the facade.

<center>
    ![](../img/a3/sa/facade.png)
</center>

Then we calculate the euclidian distance from the voxel to every facade points. Even though I did not manage to do a manifold one, the end result shall be identical.

<center>
    ![](../img/a3/sa/disfacade.png)
</center>

###  Pesudocode 

We provide Pesudocode for this part

####  The Solar envelope 

```python
# initialization
import numpy, topogenesis, ladybug, ...
load optional_envelope.obj, immediate_context.obj
read voxelized_envelope_lowres.csv & transform to envelope_lattice

# compute sum vectors
set latitude & longitute
for the first day of each season:
    for 24 hours:
        calculate sun_vector
        if sun_vector below horizontal:
            add sun_vector
rotate sun_vector by 36.324

# create sun directions and sun sources
for cen in voxel_cens:
    for sun_dir in sun_vector:
        ray_src.append(cen)
        ray_dir.append(sun_dir)
(do the same but negative to obtian ray_dir2)

# calculation
tri_id, ray_id = context_mesh.ray.intersects_id(ray_src, ray_dir, multiple_hits=False)
tri_id2, ray_id2 = context_mesh.ray.intersects_id(ray_src, ray_dir2, multiple_hits=False)

# calculate the percentage of hitting
for voxels:
    for rays:
        add hits to the corresponding voxel
    compute hits/(total rays)
    store the result

# transform to a lattice
for flattened avail lattice:
    if avail:
        append the percentage
    else:
        append 0
reshape to create a lattice

# visualization and saving
visualize
save to csv
```

####  The sky view factor 

```python
# initialization
import numpy, topogenesis, ladybug, ...
load optional_envelope.obj, immediate_context.obj
read voxelized_envelope_lowres.csv & transform to envelope_lattice

# compute sum vectors
create icosphere
if icosphere above horizontal:
    add sun_vector

# The Latter part is identical to solar_envelope file

# create sun directions and sun sources
for cen in voxel_cens:
    for sun_dir in sun_vector:
        ray_src.append(cen)
        ray_dir.append(sun_dir)

# calculation
tri_id, ray_id = context_mesh.ray.intersects_id(ray_src, ray_dir, multiple_hits=False)

# calculate the percentage of hitting
for voxels:
    for rays:
        add hits to the corresponding voxel
    compute hits/(total rays)
    store the result

# transform to a lattice
for flattened avail lattice:
    if avail:
        append the percentage
    else:
        append 0
reshape to create a lattice

# visualization and saving
visualize
save to csv
```

#### The Distance to Facade

```python
# initialization
import numpy, topogenesis, ...

# load lattice
read lattice.csv & transform to lattice

# find the facade
define stencil

insider = []
for i in lattice.find_neighbors(stencil):
    inside = True
    for n in i:
        if n is not avail:
            inside = False
    if inside:
        insider.append(i)

facade = avail_lattice
for i in insider:
    facade[i[0],i[1],:] = False

# calculate distance to facade
define lattice_cens, facade_locs

dist_m = []
for voxel_cen in lattice_cens:
    dist_v = []
    for facade_cen in facade_locs:
        dist_v.append(distance voxel_cen to facade_cen)
    dist_m.append(dist_v)
min_dist = np.array(dist_m).min(axis=1)

make min_dist a lattice: envelope_eu_dist_lattice
envelope_eu_dist_lattice += 1

# visualization and saving
visualize
save to csv
```
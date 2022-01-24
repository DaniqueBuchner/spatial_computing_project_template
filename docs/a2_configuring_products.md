# Configuring

## Table/Matrix of relations
The matrix table with connectivity between spaces has been made through a matrix diagram:
<iframe src="https://docs.google.com/spreadsheets/d/1TXr1Tw5Gzu26ivqIP-iSf-n2fq-23OCEq1nCrT7dKM0/edit#gid=1682179560" style="width:200%; height:600px;" frameborder="0">
</iframe>

#### List with the numbers and the corresponding facilities:

0 Student housing (1 person)
1 Student housing (4 persons)
2 Assisted living
3 Starter housing
4 Parking spaces
5 Bicycle parking
6 Vegetation
7 Workshops
8 Fab-labs
9 Co-working spaces
10 Start-up offices
11 Library
12 Cinema
13 Café
14 Arcade
15 Living room
16 Co-cooking
17 Restaurant
18 Community centre
19 Shop
20 Gym
21 Coffee corner

<table><thead><tr class="header"><th>Numbers</th><th>Facilities</th></tr></thead><tbody><tr class="odd"><td>0<td>Student housing (1 person) </td></tr><tr class="even"><td>1</td><td><p>Student housing (4 persons)</p></td></tr></tbody><tr class="odd"><td>3<td>Starter housing </td></tr><tr class="even"><td>4</td><td><p>Parking spaces</p></td></tr></tbody><tr class="odd"><td>5<td>Bicycle parking </td></tr><tr class="even"><td>6</td><td><p>Vegetation</p></td></tr></tbody><tr class="odd"><td>7<td>Workshops</td></tr><tr class="even"><td>8</td><td><p>Fab-labs</p></td></tr></tbody><tr class="odd"><td>9<td>Co-working spaces </td></tr><tr class="even"><td>10</td><td><p>Start-up offices</p></td></tr></tbody><tr class="odd"><td>11<td>Library </td></tr><tr class="even"><td>12</td><td><p>Cinema</p></td></tr></tbody><tr class="odd"><td>13<td>Café </td></tr><tr class="even"><td>14</td><td><p>Arcade</p></td></tr></tbody><tr class="odd"><td>15<td>Living room </td></tr><tr class="even"><td>16</td><td><p>Co-cooking)</p></td></tr></tbody><tr class="odd"><td>17<td>Restaurant </td></tr><tr class="even"><td>18</td><td><p>Community centre</p></td></tr></tbody><tr class="odd"><td>19<td>Shop </td></tr><tr class="even"><td>20</td><td><p>Gym</p></td></tr></tbody><tr class="odd"><td>21<td>Coffee corner </td></tr>







> Here you should include the process and product of your 2nd activity: **Configuring**

<table><thead><tr class="header"><th>Title</th><th>Configuring (process): Circulation Manifold (product)</th></tr></thead><tbody><tr class="odd"><td>Objective</td><td>Formulate a spatial (topological) concept, design a modular circulation manifold on a pixel/voxel grid.</td></tr><tr class="even"><td>Procedure</td><td><p>Construct a voxelated model of the site with a maximum height of 100 meters. Orient the voxel grid to a global coordinate system (e.g. geographical North-East-West-South). Size the voxels carefully based on the modular height of steps and the length of stair flights and ramps so that they fit in X/Y directions into multiple pixels. Choose the Z size of voxels according to step risers and choose the same size for X and Y as a whole multiple of step threads.</p><p>There are three types of spaces in terms of pedestrian movement in buildings, metaphorically speaking, spaces to <strong>walk</strong> through (e.g. corridors, ramps, and stairs), spaces to <strong>stand</strong> on (e.g. platforms connecting doors to corridors and stairs) and spaces to <strong>sit</strong> on (functional rooms/spaces). Construct a simplified mesh model of all bridges (corridors, ramps, stairs) connected by standing platforms in a modular grid of voxels/pixels. Take into account the free-height necessary for all spaces and pack them into the bounding volume of the building. For every functional space, leave a single pixel as a standing platform and colour it with the corresponding colour.</p></td></tr></tbody></table>
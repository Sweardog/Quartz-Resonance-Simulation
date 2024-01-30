# Quartz Resonance Simulation in Blender

## Overview
This is a "magic quartz crystal" acoustics simulation, designed to calculate the optimal height to cut a specific quartz shape allowing for maximum longitudinal confocal resonance. The real-world intent is to resonate a small piece of this quartz with hypersonic waves in a ripple-tank-like fashion and produce a precise piezoelectric frequency.

## Scripts Included
- **"Centroid Alignment"**: Uses a centroid gap minimization algorithm to compute the optimal height for quartz resonance.
- **"Generate Quartz"**: Given desired dimensions, creates a beveled para-cyldinrical quartz mesh object using Blender bmesh. 
- **"Animate Waves"**: Generates cross-sectional waves and keyframes the wave animations.

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/Scripting%20Workspace.png" width="400">

## How to Run the Scripts in Blender
1. Ensure you're within the "Scripting" workspace.
2. At the top of the scripting panel, you'll notice "Centroid Alignment" as the active text file.
3. To the left of its name, there's a dropdown menu where you can select and switch between the provided scripts.
4. Adjust the input parameters to your desires. The comments next to the variables located within the python main function should well describe their purpose (e.g. certain dimensions, amounts, velocities, orientations, etc.)
5. Click the "run script" play button. Beforehand, I usually click on the main "Scene Collection" to make it the active collection such that, for example, a generated collection or mesh object spawns in the scene collection.

## Explanations Begin

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/ChangingParabola.gif" width="400">

The parabola $y = -\frac{x^2}{2h} + \frac{h}{2}$ plays a role in the shape of the quartz and posesses significant reflection abilities:
- The [**focus**](https://www.varsitytutors.com/hotmath/hotmath_help/topics/focus-of-a-parabola#:~:text=A%20parabola%20is%20set%20of,of%20symmetry%20of%20the%20parabola.) of this parabola is **always** the origin, $(0, 0)$, regardless of the real number, height $h$, where $h$ is twice the distance from the origin to the $x = 0$ point of the parabola ($h$ is the height of the quartz - you'll see why soon). The above visual shows rays emanating radially away from the origin, reflecting with direction vectors precisely equal to $[0, -1]$
- Angle of incidence equals angle of reflection. Hence, when a horizontal line is projected at the parabola vertically from below, every point of line reflects to the origin.

Proving the reflections is relativelty easy while knowing the equation of a parabola's focus, but I wanted a deeper understanding, so I wrote a ground-up proof of the reflections here: https://www.overleaf.com/read/nhgdyvwfctvz

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/2D%20Oscillation%20GIF.gif" width="400">

Likewise, the reflection vectors equal $[0, 1]$ upon the same points casted towards the symmetrical lower parabola, $y = \frac{x^2}{2h} - \frac{h}{2}$. Thus, if a wave is enclosed by the upper and lower parabolas, the wave will oscillate between radial and vertical trajectory.

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/3D%20Oscillation%20GIF.gif" width="400">

If instead spherical waves are bouned by these dual symmetric paraboloids, this concept extends from $\mathbb{R}^2$ to $\mathbb{R}^3$. 

## Longitudinal Oscillator Shape

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/2D%20Quartz%20Shape.png" width="400">

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/Quartz%20Shape%20GIF.gif" width="400">

Above lies the shape of the "magic quartz crystal". The quartz's upper and lower portions are sculpted in alignment with the upper and lower paraboloids, while its outer form is circumscribed to a narrow cylindrical radius. Circular beveling is used to smoothly connect the paraboloids to the cylinder's lateral surfaces.

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/HTAN.png" width="400">

A conic projection of angle $\theta$ above the $-z$ axis will intersect the paraboilds at radius $h*tan(\dfrac{\theta}{2})$. In perfect conditions, the maximum parabolic radius of the quartz should be set to the maximum trajection angle, which for me is $24.55^\circ$. However, in the real world, a bit of grace-room is added to ensure the waves are fully absorbed by the paraboloids. Thus, the blending actually starts at the half angle ($27.25^\circ$) between the trajection angle ($24.55^\circ$) and the angle which hits the side edges of the quartz ($29.95^\circ$). However, under the conditions of a perfect simulation, I enjoy placing the starting bevel angle at the maxiumum projection angle because it's visually appealing.

## A Homogeneous Resonance Longitudinal Oscillator

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/HomogeneousDisplacement.gif" width="400">

For a comprehensive understanding of this simulation, one can initially start with the oscillator shape, made of no tangible material, yet endowed with reflective and transmissive properties. Envision conic segments of spherical sound waves frequently emitted from the center of this configuration, subsequently giving rise to equidistant displacements amongst the waves bounded within the paraboloids.

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/HomogeneousAligned.gif" width="400">

Scaling the height $h$ of the paraboloid configuration allows the waves to line up. When firing waves, if the first wave is ahead of the second, scale larger, otherwise if it's behind, scale smaller. These waves travel at a constant velocity in the $z$-direction. Thus, to scale the object, we take the gap distance between central tips of the first two waves and then accordingly shave or add the half the distance from the top and bottom, causing the waves to align. Elaborating, if half the tip displacement is taken or added $h$, then after one oscillation, the first wave travels less or more by the full distance, $h/2$, to or from the bottom and top combines to $h$ per full wave cycle, thereby aligning the two waves.
 
## An Inhomogeneous Resonance Oscillator (The Challenge)

If only calculating the resonance height of the quartz is as easy with quartz...

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/GrowingQuartz.gif" width="400">

Quartz (in this case, the usual untwinned alpha quartz) most naturally grows in concentric layers of a pointed hexagonal prism. The pointed hexagonal prism shape of quartz crystals arises from their intrinsic lattice structure and growth habit, which naturally minimizes the crystal's energy during formation. Thus, quartz grows at different density rates in different directions. This anisotropic density creates inhomogeneous velocities inside the quartz when wave formations emit within. 

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/Spherical%20Velocities.gif" width="400">

The above GIF displays the inhomogeneous nature of a wave with color-coded velocities.

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/InhomogeneousDisplacement.gif" width="400">

Above are what the waves look like if they are emitted within the quartz as opposed to no tangible material. Aligning these waves becomes a bit trickier because of their distortion.

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/InhomogeneousAligned.gif" width="400">

Due to the distortion, a number of waves are chosen to be aligned by their centroids, not necessarily their "centers". Aligning the waves this way as opposed to their centermost tips allows for a greater longitudinal stress force. In my case, the height calculates to approximately $4.913\text{ }cm$

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/SphericalVelocitiesPlanar.gif" width="400">

Above are planar particles of a wave, again with color-coded velocities.

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/Phase%20Cancel.gif" width="400">

Above are subsequent rings of particles traveling at the average planar velocity, $5877700 \dfrac{m}{s}$, initially emitted at the same frequency as our conic trajections. I'm showing this to emphasize the cylindrical radius of the quartz is set for phase cancellation along the planar trajection. Not only is our aim is to resonate the quartz vertically. We also want it to not resonate horizontally. This calculation is much easier than finding the height of the quartz. The average frequency of the planar particles, which is $638800 \text{Hz}$, is set equal to $\dfrac{7r}{5877700 \dfrac{m}{s}}$ where $r$ is the cylindrical radius of the quartz. The first wave travels $7$ radii by the time the second wave hits the quartz, and the waves are as far apart within the quartz as possible (they intersect at half the radius), ensuring they destructively interfere. 

## Inhomogeneous Velocity Re-Mapping Within the Quartz

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/BetaTheta.png" width="400">

With the aim to align waves by their centroids now in place, the functions that govern the wave's distortion will first be explained. After measuring the velocity of the hypersonic waves within the quartz along the $x, y$ and $z$ axes, the velocity remapping function simply becomes a function whose input is a direction vector. Suppose a particle of a wave has a direction vector $\vec{d}$, whose planar angle is defined by $\beta$ and polar angle by $\theta$.

Here, the velocity transformation is a piecewise function defined as:

$$vel = ((\dfrac{6\beta}{\pi})B + (1 - \dfrac{6\beta}{\pi})A)(1 - \dfrac{2\theta}{\pi}) + C(\dfrac{2\theta}{\pi})$$

for odd integers $n$, and

$$vel = ((\dfrac{6\beta}{\pi})A + (1 - \dfrac{6\beta}{\pi})B)(1 - \dfrac{2\theta}{\pi}) + C(\dfrac{2\theta}{\pi})$$

for even integers $n$, where the condition 

$$(n-1)\dfrac{\pi}{6} \leq \beta \leq (n)\dfrac{\pi}{6}$$

is satisfied. To obtain $vel$, one must first find $vel_{xy}$, the velocity within the $xy$-plane, which is a function of only $\beta$.

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/PlanarLabels.png" width="400">

To find this planar velocity, first take a look at the above image, which describes the hexagonal quartz unit cell in the $xy$ plane. Imagine dividing the $xy$-plane into intervals of $\dfrac{\pi}{6}$ radians, which is the same as $30^\circ$ intervals. At these specific radian multiples, switching occurs between two values: $A$ and $B$, which are the velocities of the waves in the quartz at their assigned directions. For me, it has been measured that $A = 5749460 \dfrac{m}{s}$ and $B = 6005940 \dfrac{m}{s}$. Here's the general logic behind how $vel_{xy}$ is determined:

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/2DnumberGIF.gif" width="400">

- If aligned with a specific direction corresponding to value $A$, the magnitude becomes $A$.

- If aligned with a direction corresponding to value $B$, the magnitude becomes $B$.

- If the vector is pointed in between the directions of some $A$ and $B$, its magnitude is ascertained using a radial weighted average of the adjacent $A$ and $B$ values:

With angle $\beta$ characterizing the planar direction of $\vec{d}_{xy}$:
- When $\beta$ is situated above a $B$ but below an $A$, the radial weighted average computes as:
$vel_{xy} = (\dfrac{6\beta}{\pi})A + (1 - \dfrac{6\beta }{\pi})B$
- In cases where $A$ is the upper value and $B$ the lower, the weighted average inverts to:
$vel_{xy} = (\dfrac{6\beta}{\pi})B + (1 - \dfrac{6\beta}{\pi})A$

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/IntroduceC.png" width="400">

Upon obtaining $vel_{xy}$, we expand the weighted average velocity function to $\mathbb{R}^3$. This involves incorporating an additional average based on the $\theta$, the polar angle of $\vec{d}$. The $z$-axis is paired with a measured velocity of $C = 6319620\dfrac{m}{s}$.

With this, the whole velocity is expressed as:

$$vel = (vel_{xy})(1 - \dfrac{2\theta}{\pi}) + C(\dfrac{2\theta}{\pi})$$

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/3DnumberGIF.gif" width="400">

## The Chosen Time of Centroid Calculations

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/Time%20Zeroed.png" width="400">

**Aligning the Waves**: 
For optimal alignment based on their centroids, it's important that the waves are in a vertical trajectory inside the Quartz. The longitudinal stress force here is our focal point of interest.

**Zeroing Out Method**: 
Before diving into centroid calculations, a distinct approach I've adopted is the "zeroing out" of the south-pole-tip of the earliest emitted (oldest) wave. This is executed by selecting a timeline such that its south-pole-tip aligns with the origin. This particular time is not necessary, but it proves a certain time where enough waves in question are descending vertically. The time could have been a bit before or after this time and the centroid calculations would be unaltered. 

**Time Calculations**:
The moment of this alignment, termed $t_{zeroed}$, for the oldest wave is computed as:

$$t_{zeroed} = (2*num - 1)*h/vel_{vert}$$

Where:
- $h$ is the height of the quartz.
- $num$ is the number of waves to be aligned.
- $vel_{vert}$ indicates the vertical velocity inside the Quartz, which could be $A$, $B$, or $C$, depending on the preferred orientation.

Considering this time, we can calculate the interval each younger, subsequent wave takes to reflect off the upper paraboloid and position itself when the oldest wave is zeroed. This time, $t$, for a younger wave is given by:

$$t = t_{zeroed} - i * t_{spawn} - (num - i - .5)*t_{diag} - (num - i - 1)*t_{vert}$$

Where:
- $i$ indexes the wave under consideration.
- $t_{spawn}$ is the wave's initial emission time (integer multiples of the frequency).
- $t_{vert}$ denotes the duration a wave vertex takes for vertical travel inside the Quartz from top to bottom.
- $t_{diag}$ is the equivalent for diagonal travel from bottom to top.

## The Centroid Calculations

**Centroid Location**:

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/InhomogeneousDisplacementC.gif" width="400">

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/InhomogeneousAlignedC.gif" width="400">

The centroid for each wave aligns with a specific point on the $z$ axis, leveraging the wave's inherent symmetry.

**Wave Symmetry**:

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/QuartzSegment.png" width="400">

Every wave showcases a symmetry spanning a $\dfrac{\pi}{6}$ interval range. For a more streamlined and minimalistic approach, we focus calculations on just a $\dfrac{\pi}{6}$ segment of each wave. We are to derive the $z$ component of the segment's centroids and are allowed to disregard the non-zero $x$ and $y$ components due to the symmetry of the entire wave.

##
**Read This Section If You Want To Tease Your Brain of a Precise, Analytical Centroid** 

A mathematical object that bears similarity to this distorted radial segment is the hemisphere segment spanning the planar angle from $0$ to \$pi/6$. Unlike the distorted radial segment, the hemisphere segment isn't distorted, but the centroid calculation approach remains analogous. 

For a hemisphere segment, the $z$-centroid stems from a specific summation: summing the products of the planar arc lengths and their respective heights, and then dividing this by the segment's area. The area of this segment is \$pi/3$, which equates to one-sixth of a hemisphere's area. Transitioning this summative concept to calculus, the infinite summation is represented by an integral:

$$z_{\text{centroid}} = \frac{\int_{0}^{\frac{\pi}{2}} (-cos(\theta)) (sin(\theta) \dfrac{\pi}{6}) d\theta}{\dfrac{\pi}{6}} = -\dfrac{1}{2}$$

Extending this centroid concept to our distorted segment in the quartz, we can determine the $z$-centroid precisely by integrating. This involves multiplying the planar strip lengths by the $z$ coordinate of each strip, and then dividing the result by the entire segment's area (the non-multiplicative integral (area)). In this context, the "arc lengths" for the distorted wave are not standard circular arc lengths and would necessitate an alternate integration technique for the length of each strip. The length of a strip can be precisely defined by an integral of the wave function mapping of the hemisphere segment. It would be a double integral over an integral for the centroid.

I've ventured into this complex calculation of integral defined planar strips using Google Colab and sympy, but to no avail. The calculus quickly becomes convoluted. If anyone can tackle and solve this, you have my applause, and a **grand prize of $1000$ HEX crypto.** I chose this currency due to the quartz unit cell shape. Message me if you can solve the centroid integral, and the token is yours.
##

A much easier, and still quite accurate, method to determine the centroid involves a numerical approximation. This technique uses the summation of the center of each face (polygon of the wave) multiplied by the area of each face. The formula for the centroid of the quartz segment is:

$$\bar{z} = \dfrac{\sum_{i = 0}^{n-1}(z_iA_i)}{\sum_{i = 0}^{n-1}(A_i)}$$

Where:
- $n$: Total number of faces of the wave segment mesh.
- $z_i$: z-coordinate of the center of the $i^{th}$ face.
- $A_i$: Area of the $i^{th}$ face.

Using this formula on a dense southern hemisphere segment mesh, the $\bar{z}$-coordinate computed is $\bar{z} = -0.4999976318645241$. Remember, the exact analytical solution is $-0.5$. By comparison, Blender's built-in centroid calculation (termed 'center of mass surface') outputs $\bar{z} = -0.49998000264167786$, demonstrating a slightly inferior accuracy to the manual computation.


## Pseudocode for Centroid Alignment (Python Script 'Centroid Alignment')

    - Set a number of waves to align
    - Assume their average gap hasn't been minimized 

    While the average gap hasn't minimized:
        for each wave:
            - Plot the wave at the timestamp where the oldest wave is zeroed
            - Record the centroid of this wave

        - Evaluate the distance gaps between adjacent centroids
        - Determine the average of these gaps

        if the average gap is positive:
            - Raise height by + |average|/2
        elif average gap is negative:
            - Lower height by - |average|/2
        else:
	        - terminate 

(**Note**: In the rare event the average gap equals zero, the while-loop terminates.) 

The second-to-last stored height serves as the final answer. Concurrently, the second-to-last stored gap indicates the minimized centroid displacement. The stored gaps between centroids of consecutive waves aren't equidistant, unlike the homogeneous oscillations. Thus, the height adjustments for the paraboloids are aligned with this average to ensure all gaps are accurately represented. The while-loop is designed to halt its process once a disparity larger than the previously recorded gaps between centroids emerges. With velocity $C$ as vertical, the computed height $h$ comes to be around $4.913 cm$

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/MinimizationChart.png" width="400">

Above is a minimization chart of the quartz height gap minimization algorithm. 

# Enhance Resonance w/ Reflectors

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/Reflections.gif" width="400">

Silver reflectors may used to enhance resonance.

## Alternative Orientations

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/ShatterOther.gif" width="400">

We have so far only kept the quartz axes in alignment with its standard orientation. That is, $z$ is vertical with $x$ and $y$ flat. However, quartz has different oscillation modes relative to its internal orientation. Suppose we start with the hexagonal prism shape, and then cut the quartz at a sideways orientation. 

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/SphericalVelocities2.gif" width="400">

Thus, the waves inside the quartz travel in a different manner. It simply boils down to a trivial swapping of cartesian coordinates within the $\vec{d}$ directional input vector to the velocity re-mapping function.

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/InhomogeneousDisplacementCOther.gif" width="400">

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/InhomogeneousAlignedCOther.gif" width="400">

For this new orientation, the with $A$ treated as the vertical velocity, the waves are instead symmetric about a $\dfrac{\pi}{2}$ segment interval. The centroid calculation still performs flawless minimization of centroids and precisely aligns the wave segments. The computed height for this alternative orientation is found to be $4.558 \text{ } cm$. 

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/PhaseCancelQuarter.gif" width="400">

**Note** The cylindrical radius is set for quarter-phase cancellation, not perfect phase cancellation. If it were perfect phase cancellation, this condition requires the quart's radius to be quite large, making it more difficult to oscillate longitudinally. As a sacrifice, quarter phase cancellation is chosen as a "middle route" optimization choice. The waves don't cancel as well, but this ensures the longitudinal waves create a great enough stress force.

## Utility

<img src="https://github.com/Sweardog/Quartz-Resonance-Simulation/blob/master/Visuals/Modulation.gif" width="400">

In reality, there is one silver reflector placed beneath the quarts and a device designed to modulate the waves in between. This modulation gives information to each wave. The functionality of this system behaves like an AM radio.

## Precision in Wave Projections

In the context of Blender, one might naturally assume the usage of the `ray_cast` function for projection tasks. While `ray_cast` has shown commendable precision, especially with high-density target meshes, my objective was to rely solely on the innate accuracy of float precision, rather than mesh density. To achieve this, I constructed mathematical line parametrization functions dedicated to precise wave projections. These functions directly map the vertices of wave meshes. For those keen on understanding the underlying math, I have embedded comments detailing the function derivations within various Python functions. Among all formulations, one that stands out is the wave projection from a spherical emitter onto the lower paraboloid. This involved determining the intersection magnitude of the cone and paraboloid, $h*tan\dfrac{\theta}{2}$, as mentioned earlier.

## Wave Dynamics Over Time 

The animation of waves hinges on meticulous computations at every collision juncture. From each collision, vital data is extracted, facilitating the projection of the wave mesh vertex to the succeeding collision point. This process persists recursively through each collision. Within the quartz's confines, the paraboloids' intrinsic characteristics cause the direction vectors to oscillate in a trivial manner. For non-trivial reflections, the normal vectors at collision points are anayltically solved for, **independent of mesh data!**. Once the gap between consecutive collision points and the corresponding velocities are determined, we possess the dataset necessary for an accurate projection. This principle integrated in conjunction with a recursive algorithm in the "Animate Waves" Python script allows for uninterrupted wave animation until a set frame limit is reached.

## Contributions

In the real-world, these simulated answers are not %100 accurate, but it provides a good place to start. Because these oscillators are cut in the centimeter range, with nothing too precise needed in terms of precision, it's relatively cheap to make these. However, if a giant quartz crystal were cut to oscillate at perhaps a lower frequency and there is only one chance to get it right due to resource/financial limits, this simulation could benefit from real-world implementations to add more realism. Once again, if someone obtains a precise centroid calculus solution, you will recieve a grand prize of $1000$ HEX crypto. 

		













































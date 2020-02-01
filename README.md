# SFND Radar Target Generation And Detection
In this project, we will 

1. Generate a TX radar signal then add a delayed for the RX response
2. Mix the TX and RX signal
3. Analyze the mixed signal using Fast Fourier Transform (FFT) and the CFAR technique.

We will use Matlab to generate the following graphs:

1. FFT

![FFT](https://raw.githubusercontent.com/randcode-generator/SFND_Radar_Target_Generation_And_Detection/work/image/figure1.jpg)

2. FFT 2D

![FFT 2D](https://raw.githubusercontent.com/randcode-generator/SFND_Radar_Target_Generation_And_Detection/work/image/figure2.jpg)

3. CFAR 2D

![CFAR 2D](https://raw.githubusercontent.com/randcode-generator/SFND_Radar_Target_Generation_And_Detection/work/image/figure3.jpg)

# Implementation
__*Nr*__ - # of range cells

__*Nd*__ - # of doppler cells

__*Tr*__ - Training Cells for range estimation

__*Td*__ - Training Cells for doppler estimation

__*Gr*__ - Guard Cells for range estimation

__*Gd*__ - Guard Cells for doppler estimation

## Implementation steps for the 2D CFAR process
1. Create a `RDB2` new array of size `Nr/2` and `Nd`. This array will be used for the final graph shown in CFAR 2D.
2. Create first loop `i` from 1 to (Nr/2)-(Gr+Tr+1)
3. Create second loop `j` from 1 to Nd-(Gd+Td+1)
4. Get the `noise_level` by using `sum(RDM_org(i:i+Tr-1,j:j+Td-1), 'all')`. `RDM_org` are values before it was converted to dB
5. Get the `threshold` by normalize `noise_level` by dividing `(Tr+Td)` then use `pow2db` to convert it dB.
6. Add `offset` to `threshold`
7. If the threshold value is greater than `RDM(i+Tr+Gr,j+Gd+Td)`, then `RDM2(i+Tr+Gr,j+Gd+Td)` should be assigned a `1`

## Selection of Training, Guard cells and offset
The `threshold` value depends on the `Tr`, `Gr`, `Td`, `Gd`, and `offset` values.

The values used in this program are `Tr`=`Td`=8, `Gr`=`Gd`=4, and `offset`=1.

The range values are between 108 and 112 which is close enough to the initial position 110.

## Steps taken to suppress the non-thresholded cells at the edges
Create a new array initialized with zeros called `RDB2` to hold values for the CFAR 2D graph
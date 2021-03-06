---
layout: post
title: experience from fundamental matrix
tags: ComputerVision
date: 2016-11-23 09:32:00
postPatterns: 'circuitBoard'
---

An example of the fundamental matrix and the epipolar line, please refer to 
[fundamental matrix coding](https://github.com/beandrewang/computer_vision/tree/master/georgiatech_cs4495/ps3_octave)

# Fundamental matrix

It is an approach of finding the epipolar line in stereo. Epipolar line is a 
line that you can find the corresponding points or features along this line 
between image A and image B. 

Suppose I know a point in image A, how to find the matched points in image
B? Find the epipolar line. 

Suppose I already know some corresponding points (at least 8) between two 
images, A and B. I would solve the fundamental matrix by the following equation.

$$
P'^{T}FP = 0
$$

$$
\begin{bmatrix}
u' & v' & 1 \\
\end{bmatrix} \
\begin{bmatrix}
f11 & f12 & f13 \\
f21 & f22 & f23 \\
f31 & f32 & f33 \\
\end{bmatrix} \
\begin{bmatrix}
u \\
v \\
1
\end{bmatrix} = 0
$$

if we change the patten it would be like this. 

$$
\begin{bmatrix}
u'u & u'v & u' & v'u & v'v & v' & u & v & 1 \\
\end{bmatrix} \ 
\begin{bmatrix}
f11 \\
f12 \\
f13 \\
f21 \\
f22 \\
f23 \\
f31 \\
f32 \\
f33 \\
\end{bmatrix} = 0
$$

how to resolve the F matrix? Two ways. 

1.  svd, use at least 9 corresponding points and then do svd with the matrix 
generated by the 9 corresponding points. The F would be the last colume vector 
of V. 

2. least square, set the f33 as 1. And we would get a different equation as 
below: 

$$
\begin{bmatrix}
u'u & u'v & u' & v'u & v'v & v' & u & v \\
\end{bmatrix} \ 
\begin{bmatrix}
f11 \\
f12 \\
f13 \\
f21 \\
f22 \\
f23 \\
f31 \\
f32 \\
\end{bmatrix} = -1
$$

solve it by least square, in this way, we has to have 8 corresponding points or 
more. 

in matlab, it is very easy, 

```matlab
F = P \ -ones(8, 1);
```

once we get the F by svd or least square, the rank of F is 3, but the corrent F
should be have rank of 2. 

How to reduce the rank of F? svd. 

## rank 2 

Set the 3rd singular value 0 and then get the new F, in matlab, 

```matlab
[U D V] = svd(F);
D(3, 3) = 0;
F = U * D * V';
```

Then reshape the F to a 3x3 matrix; in matlab

```matlab
F = reshape(F, 3, 3)';
```

# epipolar line 

$El = FP$ is the epipolar line. This is a vector in homogeneous coordinate? We 
cannot draw this line by the regular way of two points to a line. 

## how to draw the epipolar line 

In a image, we can get the intersection points of the epipolar line with the 
left and right boundary lines of the image. 

Suppose I the image size is w x h,

### left and right boundary lines 

up-left point A is $(1, 1)$

bottom-left point B is $(1, h)$

up-right point is C $(w, 1)$

bottom-right point D is $(w, h)$

then, 

left boundary line Ll is $A \times B$

right boundary line Rl is $C \times D$

### the intersection points between epipolar line and the boundary lines

intersection point with left boundary line Pl is $El \times Ll$

intersection point with right boundary line Pr is $El \times Rl$

We obtain two points now then draw it. 

### draw the epipolar line

Though we get the two point, they are still in homogeneous coordinate, we have 
to convert to inhomogeneous coordinate first. 

convert the points from homogeneous coordinate to inhomogeneous coordinate, in
matlab, 

```matlab
Pl = [Pl(1) / Pl(3), Pl(2) / Pl(3)];
Pr = [Pr(1) / Pr(3), Pr(2) / Pr(3)];
```

then plot the line, it is easy in matlab. 

```matlab
x = 1 : w;
y = Pr(2) + (x - Pr(1)) * (Pl(2) - Pr(2)) / (Pl(1) - Pr(1));
plot(x, y, 'color', 'green');
```

# Additional

Normally, once we get the corresponding points in two images, normalized them 
first, make the mean be 0, and the maximum is 1 and minimum is -1 or not too far
from the 1 and -1. 

```matlab
function [normpt T] = norm_pt(pt)
    cu = mean(pt(:, 1));
    cv = mean(pt(:, 2));
    
    s = 1 / std(pt(:));
    
    T = [s 0 0; 0 s 0; 0 0 1] * [1 0 -cu; 0 1 -cv; 0 0 1];
    normpt = T * [pt ones(size(pt, 1), 1)]';
    normpt = normpt(1 : 2, :)';
end
```

Then use the normalized points to compute the Fnorm and then convert the Fnorm 
to F. 

```matlab
F = Tb' * Fnorm * Ta;
```

Then use this F to draw the epipolar line. 

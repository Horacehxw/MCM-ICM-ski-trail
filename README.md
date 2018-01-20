# MCM-ICM-ski-trail

Data available from [google elevation API](https://developers.google.com/maps/documentation/elevation/start)

Need a [poltly](https://plot.ly/) acount for fancy visulization.

## Step 1: Use Google Elevation API to download data.

## Step 2: Transform raw data into 2D array map format.

## Step 3: Ski Trace Searching Algorithm
```Python
def Search(Point a, step r, slope s):
    find all the point(in 36 different direction) on the cycle centered at a, radius = r.
    claculate the slope
    If (all slope is negative):
        return
    Else:
        find the 3 point with slope closet to s
        a.record(b,c,d)
        search(b),search(c), search(d)
```
Slope is $tan(\theta)$, which is the ratio between horizontal and vertical distance.

Optimization:

* Keep tracking the variance of slopes on each ski track, terminate the search in advance for those with high variance.

* recurrence with memorization to avoid repited computation.

## Step 4: Random Start Searching, Minimize the Cost function.

Among the paths we randomly searched, our goal is to find those satisfy two objective:

* The trail slope is close to the given area

The hinge loss is applied, there's a linear penalty for the slopes exceeding:

$$
f(i)=
\begin{cases}
k-S_{i} &amp; \text{ if } S_{i}&lt;k \\ 
\infty &amp; \text{ if } S_{i}&gt;k' 
\end{cases}
$$

$$ L_1 = \sum_{i=1}^{N}f(i)$$

* The terrian roughness is as small as possible

We use the standard deviation of slopes on the path:

$$L_2 = \sigma_{a} ={\sqrt {{\frac {1}{N}}\sum _{i=1}^{N}(S_{i}-\bar{S})^{2}}},{\rm {\ \ where\ \ }}\bar{S} ={\frac {1}{N}}\sum _{i=1}^{N}S_{i}$$

In order to get the overall loss function, we normalize each of above objective and add them together.

### 4.1,2,3 Find Optimal paths for easy,intermidiate,hard trail

randomly choose 5000 starting point and find the coresspoing paths.

Find 10 best path with the smallest cost

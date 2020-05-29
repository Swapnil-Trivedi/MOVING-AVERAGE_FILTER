# MOVING_AVERAGE_FILTER
Implementation and study of a Moving Average Filter to remove noise from sensors used in industrial appliacations.

## INTRODUCTION
Moving Average Filter : The moving average filter is a simple Low Pass FIR (Finite Impulse Response) filter commonly used for regulating an array of sampled data/signal. It takes M samples of input at a time and takes the average of those to produce a single output point.

#### NEED FOR A FILTER
Before understanding the need for the filter we need to understand how the device or sensor actually work
##### KEY TERMS :
- Sample Rate : sample rate is the rate at which the sensor collect/record a data value.
- DATA frame  : A data frame is a collection of samples(data) recorded over a particular time range.
- True Value  : True valuse is the absolute value without any error in it.
- Device Error: The measuring error of a device or simply the rate at which the sample taking property of the device fluctuates.
- Measured Value : The value sensed by the sensor which may be subject to noise.
```
TRUE VALUE = MEASURED VALUE - DEVICE ERROR
```
so how a sensor actually work is, our sensor comes with a default sample rate and when installed the sensor start sending the data frames to the controller for calculations or further manipulations. The data frame is used because if a single sample is used to estimate the true value it may lead to terrible state estimations. it's done in order to avoid the noise in our sensor's data injected by the frequency of operation and vibrations of surrounding machines in an industrial environment.
But not even our Data frame is prone to noise injection and in order to find out the underlying signal we need a system for our state estimation.
###### Hence we need a filtering system that can reduce error in our data frame and provide with a precise and close approximation.

## IMPLEMENTATION

#### SCENARIO

let's take example of a flow meter. A flow meter is a device use to measure the volume and rate of flow of fluid from a pipeline.let's assume a situation where the actual rate of flow of fluid in the pipe is unknown and our sensor comes with the following specifications.

##### specifications : 
- Device Error - 1% of Measured Value
- Sample Rate  - 1 sample/sec
- Framing Time - 1 minute i.e time over which a Data frame is generated.

In the given scenario our sensor have a sample rate of 1 and framing time of 1 minute, hence we will end up with a data frame of 60 samples every minute.
let's assume that our flow meter needs to measure a rate of 75 litre/meter. what actually happen is in an interval of 60 seconds the sensor will generate 60 individual data points or samples all of which might be subject to noise.
so after collecting samples and forming a data frame our sensor sends the data to a controller for further manipulation.

#### Here's what our Raw data looks like 

![](/ACTUAL_DATA_VS_NOISE.png)
Well from that we can clearly say that our data frame is far from the actual data we need to calculate. if it was not for the red line we would have never guessed the underlying data.
so in order to clean up our data frame and to come to single point of approximation we apply a single mathematical approach of error reduction.
we use a MOVING AVERAGE FILTER

##### The process is fairly simple but very impactful
- we take our samples individually start iterrating through them
- calculate the sum of individual sample as we iterrate 
- find the average for each data point

or to sum up we simply follow this mathematical approach
![](/formula.png)

##### Well let's code it up 
```

# MOVING AVERAGE FILTER this cell here is our entire filter function
#so we do a step by step increment in our average value and try to plot the data
# so every time we take a data we get average of it
average=0
suml=0
avglist=[]
n=1
for i in range(0,60):
    suml=suml+data[i]
    average=(suml)/n
    avglist.append(average)
    n=n+1


```
so after passing our data frame from our SMA (Simple Moving Average) Filter we obtain our estimation matrix which visually looks ssomething like this :
![](/APPLICATION_OF_MOVING_FILTER.png)
well that's not magic but plain old MATH!!
so we can clearly say that our samples here are much more organized and for any set of value it will try to establish a point of singularity or approximation with less error.The [script here](/FILTERS.ipynb) shows a step by step of implementing the filter. 

since i have used a random function to stimulate the inputs from a sensor the wworking of the filter can be verified every time you run the script, every single time the filter helps in finding the closest approximation for our value.

But still we haven't taken device error in account because that depends from device to device and also from the manufacturer.

##### so what happens when our device/sensor actually have a device error or a measurement error???
well plain old math again we simply subtract the device error from the measured value to get our true value.

let's just visualize that too. let;s assume that the above mentioned flow meter comes with a device error of 1% so we simply take the 1% of a sample and subtract it from the sample itsself, now let's feed the new data frame (the more correct one ) to our filter.

And this is what we have. our end result is a completely( well almost there) free data which can be used in a multiple way.
![](/ERROR_CORRECTION_IN_SMF.png)
well in order to generate a better point of estimation or approximation many mdern sensors comes with an unbelivable sample rate, well because more sample rate means more data and the more there is the better error reduction is possible.

### CONCLUSION
We Implemented a FIR (Finite impulse response) Filter which can reduce noise from our sensor data and provide with a much more stable and error free state estimation for the controller.

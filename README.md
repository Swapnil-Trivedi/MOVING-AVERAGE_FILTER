# MOVING_AVERAGE_FILTER
Implementation and study of a Moving Average Filter to remove noise from sensor used in industrial appliacations.

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
Well from that we can clearly say that our data frame is far from the actual data we need to calculate. if it was not for the red value we would have never guessed the underlying data.
so in order to clean up our data frame and to come to single point of approximation we apply a single mathematical approach of error reduction.
we use a MOVING AVERAGE FILTER

##### The process is fairly simple but actually is quite impactful
- we take our samples individually start iterrating through them
- calculate the sum of individual sample as we iterrate 
- find the average for each data point
or to sum up we simply follow this mathematical approach

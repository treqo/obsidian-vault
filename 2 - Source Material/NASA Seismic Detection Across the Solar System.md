**Type:** [[competition]]  
**Topics:** #  
**Date:** 2024-10-05  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- 

---
# Link Tree

- https://www.spaceappschallenge.org/nasa-space-apps-2024/challenges/seismic-detection-across-the-solar-system/?tab=details

Submission Idea:

- GitHub Repo (read me and code)
- 5 slide presentation showing the processing of large data set in steps (models used and filtration)
- more use a 2 min video with animations (Manim etc) on the process 
- A more indepth document (Could be the README)

# Problem Statement

Only a fraction of data is useful and sending over data is power expensive. Distinguish seismic quakes from noise and send it.

There's a **difficulty** in transferring high-resolution seismic signals back to Earth. *Amount of power scales with distance*.

QUAKES ARE RARE (not common in all the data)

This constraint is especially important as seismologists will likely be **sharing** the lander with science teams from other disciplines who have different objectives and instruments, some of which may be transferring even larger amounts of data to Earth. Consequently, **data is recorded at lower resolution** or with fewer instruments than might be optimal to achieve the desired science.

run algorithms on a lander to **differentiate seismic data from the noise**, so that only the useful signals can be extracted and sent back to Earth

````ad-important
seismic signals on other planets tend to **look different** than on Earth, and the signal might be only *faintly observable in the noise*.

- [?] How will we make an algorithm that is planet independent?
````

# Objective

```ad-summary
Your challenge is to write a **computer program** to analyze real data from the Apollo missions and the Mars Interior Exploration using Seismic Investigations, Geodesy and Heat Transport (InSight) Lander to identify seismic events
```

```ad-important
Don’t forget to watch out for **missing data and glitches**, though—both of which are common for planetary data
- [?] How will we account for missing data and glitches? What exactly is a glitch?
```
## Answer

### Dealing with Missing Data and Glitches in Planetary Seismic Data

Planetary seismic data is often imperfect, containing **missing data** and **glitches** due to the harsh environments in which instruments operate, transmission challenges, and sensor limitations. These issues need to be accounted for to avoid misinterpreting noise or erroneous data as actual seismic events.

### 1. **Missing Data**

Missing data refers to gaps in the recorded seismic signals, where no readings were captured. This can happen for various reasons, such as:

- Instrument malfunction or temporary shutdowns.
- Data loss during transmission to Earth.
- Storage issues on the lander or rover.

**How to Identify Missing Data:**

- In a **CSV file**, missing data may appear as `NaN` (not-a-number) values or gaps where certain timestamps have no corresponding measurements.
- In **MiniSEED** files, the absence of readings for certain time intervals may be indicated by sudden jumps in time between consecutive readings or flagged metadata.

**How to Handle Missing Data:**

- **Interpolation**: You can fill small gaps in the data using interpolation techniques, which estimate missing values based on the data points surrounding the gap.
    - Example: Linear interpolation between two known data points can estimate the missing value.
    - Be cautious with large gaps, as this could introduce inaccuracies.
- **Masking**: For larger gaps, it's better to mask or remove the missing segments from the analysis, avoiding false conclusions from interpolated data over long intervals.
- **Padding**: Some algorithms (especially machine learning models) may require fixed-length data. You can pad the missing segments with zeros, but ensure the algorithm understands this padding.

### 2. **Glitches**

Glitches are **erroneous data points** caused by anomalies in the sensor or external interference. These can look like sharp, unnatural spikes in the seismic signal, often significantly different from actual seismic events. Some common causes of glitches include:

- Instrumental noise (e.g., electronic interference).
- Data transmission errors.
- Mechanical disturbances to the lander or rover (e.g., vibrations from equipment).

**How to Identify Glitches:**

- **Visual Inspection**: Glitches typically appear as sharp, isolated spikes or discontinuities in the time-series data. Unlike seismic events, they don’t follow the expected waveform pattern (no clear P-waves or S-waves).
- **Sudden Changes in Amplitude**: Glitches often cause a sudden, extreme change in amplitude for just a few data points, followed by a return to normal.
- **Unrealistic Patterns**: Look for abnormal, high-frequency noise or outliers in the data that deviate significantly from typical seismic waveforms.

**How to Handle Glitches:**

- **Filtering**: Use filters to remove high-frequency noise or anomalies. For example, a **band-pass filter** can help isolate the frequency range where actual seismic signals are expected, filtering out glitches that occur at higher frequencies.
- **Outlier Detection**: Glitches are often outliers in terms of amplitude. You can use outlier detection techniques to identify and remove or mask them.
    - Example: Define a threshold based on the average signal amplitude, and discard any points that exceed a certain multiple of the standard deviation.
- **Window-based Smoothing**: Apply a moving average or other smoothing techniques to reduce the impact of glitches. However, be cautious as too much smoothing can also blur real seismic events.
- **Manual Inspection**: If possible, visually inspect any flagged glitches to ensure they are not actual seismic events.

### 3. **Strategies to Account for Missing Data and Glitches**

1. **Pre-Processing Steps**:
    
    - Before applying any detection algorithm, clean your data by identifying and handling missing data and glitches.
    - Use interpolation or masking to address missing data, and apply filters or outlier removal techniques to handle glitches.
2. **Data Filtering**:
    
    - Apply a **band-pass filter** to isolate the frequency ranges where seismic signals occur. This can reduce the impact of both missing data and glitches by focusing on relevant frequency components.
3. **Robust Algorithms**:
    
    - If you're using an algorithm like **STA/LTA** to detect seismic events, make sure it is robust to outliers and noise. You may want to add pre-processing steps that smooth or filter the data before applying the detection algorithm.
4. **Catalog Review**:
    
    - After detecting seismic events, visually review any detections near segments with missing data or glitches to ensure they are legitimate. Glitches might trigger false positives in automatic detection algorithms, so manual verification is helpful.

### Example of What a Glitch Might Look Like:

- **In a time-series plot**, a glitch might look like an isolated, sharp peak or spike that stands out drastically from the surrounding data. It’s likely much narrower (lasting only a fraction of a second) and with an unusually high amplitude compared to a real seismic event.
- **In the frequency domain**, glitches can produce very high or erratic frequencies that don’t match the expected frequency content of a typical seismic signal.

### Conclusion:

Handling missing data and glitches is crucial to ensure accurate seismic event detection. Pre-processing the data by filtering out noise, filling or masking gaps, and removing glitches can significantly improve the performance of your detection algorithm and reduce false positives. Always verify the results, especially near problematic data segments.

## Machine Learning techniques and Algorithms

- STA/LTA (Conventional Algorithm)

## **Potential Considerations**

_You may (but are not required to) consider the following:_

- The **accuracy** of your method is important and so is **creativity**, so think out of the box!
- Planetary seismic data is **messy**, with a **low signal-to-noise ratio**. You may have to do some **signal processing** like **running high-pass filters** to make your life a bit easier!
- If your team is taking the algorithm development approach, discuss whether to design different algorithms for each dataset or a single adaptable algorithm for all data.
- If your team is taking the manual approach, think about how you could display the seismic waves in different ways. Get creative!
- These seismic wiggles can be unruly, with lots of errors. If your team is struggling, try changing the data representation.

_For data and resources related to this challenge, refer to the Resources tab at the top of the page. More resources may be added before the hackathon begins._


SINGLE ADAPTABLE ALGORITHM so it can work on any planet. Perhaps adjustable PARAMETERS set for different planets.


TYPES OF **MOONQUAKES**

- Impact
- deep moonquakesd
- shallow moonquakes

````ad-important
title: Naming the data
For your prediction, feel free to include either the **absolute time** or **relative time**, just make sure to mark it using the same header in the CSV file so we can easily score it!

example `time_abs(%Y-%m-%dT%H:%M:%S.%f)` : `xa.s12.00.mhz.1970-01-19HR00_evid00002`
````

![[Pasted image 20241005120815.png]]

```ad-important
**Note**: You do not have to worry about marking the end of the seismic trace (as you can see, even for us it's not very accurate!). For this challenge, all we care about is the start of the seismic waveform.
```

---


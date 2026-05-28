\# IV Surface Prediction



\## Current Best Score



Linear interpolation baseline public score: `0.0000423397`



\## Methods Tried



1\. Row-wise CE/PE mean fill  

&#x20;  Public score: `0.0234618566`



2\. Linear interpolation across strikes  

&#x20;  Public score: `0.0000423397`



\## Current Best Method



For each timestamp:

\- CE missing values are filled using linear interpolation across CE strikes.

\- PE missing values are filled using linear interpolation across PE strikes.

\- Remaining missing values are filled using time interpolation.


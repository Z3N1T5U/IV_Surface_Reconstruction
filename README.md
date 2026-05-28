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



| Rank | Method | Public Leaderboard MSE |
|---:|---|---:|
| 1 | Linear strike interpolation | **0.0000423397** |
| 2 | Quadratic interpolation | 0.0000649789 |
| 3 | 80% strike interpolation + 20% time interpolation | 0.0001245320 |
| 4 | 70% strike interpolation + 30% time interpolation | 0.0002294140 |
| 5 | Cubic interpolation | 0.0002751123 |
| 6 | 50% strike interpolation + 50% time interpolation | 0.0005667500 |
| 7 | Local neighbor average | 0.0011057528 |
| 8 | Time-first interpolation | 0.0021542590 |
| 9 | Row-wise CE/PE mean fill | 0.0234618566 |
| 10 | Linear interpolation with clipping | 0.0295588421 |
| - | Moneyness-based interpolation | Not tried yet |
| - | LightGBM regression | Not tried yet |
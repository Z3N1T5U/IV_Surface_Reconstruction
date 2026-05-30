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



| Rank | Method                                            | Public Leaderboard MSE |
| ---: | ------------------------------------------------- | ---------------------: |
|    1 | Linear strike interpolation                       |       **0.0000423397** |
|    1 | Moneyness-based linear interpolation              |       **0.0000423397** |
|    2 | CE = PCHIP, PE = Linear                           |           0.0000449949 |
|    3 | Quadratic interpolation                           |           0.0000649789 |
|    4 | CE = Linear, PE = PCHIP                           |           0.0000705040 |
|    5 | PCHIP interpolation                               |           0.0000731591 |
|    6 | 80% strike interpolation + 20% time interpolation |           0.0001245320 |
|    7 | 70% strike interpolation + 30% time interpolation |           0.0002294140 |
|    8 | Cubic interpolation                               |           0.0002751123 |
|    9 | 50% strike interpolation + 50% time interpolation |           0.0005667500 |
|   10 | Local neighbor average                            |           0.0011057528 |
|   11 | Time-first interpolation                          |           0.0021542590 |
|   12 | Row-wise CE/PE mean fill                          |           0.0234618566 |
|   13 | Linear interpolation with clipping                |           0.0295588421 |
|    - | ATM-aware interpolation                           |          Not tried yet |
|    - | Internal validation (masked values)               |          Not tried yet |
|    - | LightGBM regression                               |          Not tried yet |
|    - | XGBoost regression                                |          Not tried yet |


# Write Concern

Made up of w and j parameters.<br>
w - write parameter<br>
j - journal parameter

|w      | j    | Notes |
|-------|------|-------|
|1      |false |Fast, default, but small window of vulnerability|
|1      |true  |Slow|
|0      |      |Unacknowledged write, not recommended|

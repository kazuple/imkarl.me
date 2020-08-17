---
title: Transformation Rules
description: 
published: true
date: 2020-06-18T08:37:49.462Z
tags: 
editor: undefined
---

# Example Regex
| Rule Name         | Description                        | Input Pattern              | Output Pattern | Result                                                            |
|-------------------|------------------------------------|----------------------------|----------------|-------------------------------------------------------------------|
| 4-digit Extension | Transforms 4-digit extensions      | (\d{4})                    | +1425555\1     | 1234 is transformed to +14255551234                               |
| Local to E164     |                                    | 0(\d*)                     | +44\1          | Incoming number beginning with 0 has it stripped and +44 added    |
|                   |                                    | 0208051332                 | +442080513320  |                                                                   |
|                   |     14085551212                    |     (\ +14085551212)       |     \1         |     +14085551212                                                  |
|                   |     5551212                        |     (.*)                   |     \1         |     5551212                                                       |
|                   |     +14085551212                   |     \ +.*(\d{4})           |     \1         |     1212                                                          |
|                   |     4085551212                     |     ^\d{3}(.*)$            |     \1         |     5551212                                                       |
|                   |     14085551212     15105551212    |     (1510\|1408)(\d{7})    |     \2         |     5551212                                                       |
|                   |     1212                           |     ^(\d{4})$              |     \1         |     1212                                                          |
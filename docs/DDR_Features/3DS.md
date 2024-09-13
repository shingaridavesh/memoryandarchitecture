DDR4 supports multiple package options:

* Monolithic Single Die Package (SPD)
* Standard Dual Die Package (DDP)
* 3DS Package (Three Dimensional Stacks)
    * 2-High (2 Logical Ranks with single signal loads)
    * 4-High (4 Logical Ranks with single signal loads)
    * 8-High (8 Logical Ranks with single signal loads)

To support these many logical ranks, in traditional system we would need that many Chip Select signals i.e. for 8 Ranks we would need 8 indivdual Chip Selects pins, which is very costly. So unlike previous DDR generations, DDR4 uses new pins called CID i.e. Chip ID which is used along with Single Chip Select per package to select the logical rank within the package.

** 2-High Configuration

|  Ranks  |      C2     |   C1    |      C0      | Chip Select (CS_n). |
| :--------: |:-------------:| :---------:| :---------: | :---------: | 
| Logcal Rank 0 | Not Used| Not Used | 0 | Low |
| Logcal Rank 1 | Not Used| Not Used | 1 | Low |

** 4-High Configuration

|  Ranks  |      C2     |   C1    |      C0      | Chip Select (CS_n). |
| :--------: |:-------------:| :---------:| :---------: | :---------: | 
| Logcal Rank 0 | Not Used| 0 | 0 | Low |
| Logcal Rank 1 | Not Used| 0 | 1 | Low |
| Logcal Rank 2 | Not Used| 1 | 0 | Low |
| Logcal Rank 3 | Not Used| 1 | 1 | Low |

** 8-High Configuration

|  Ranks  |      C2     |   C1    |      C0      | Chip Select (CS_n). |
| :--------: |:-------------:| :---------:| :---------: | :---------: | 
| Logcal Rank 0 | 0| 0 | 0 | Low |
| Logcal Rank 1 | 0| 0 | 1 | Low |
| Logcal Rank 2 | 0| 1 | 0 | Low |
| Logcal Rank 3 | 0| 1 | 1 | Low |
| Logcal Rank 4 | 1| 0 | 0 | Low |
| Logcal Rank 5 | 1| 0 | 1 | Low |
| Logcal Rank 6 | 1| 1 | 0 | Low |
| Logcal Rank 7 | 1| 1 | 1 | Low |

** Pros and Cons
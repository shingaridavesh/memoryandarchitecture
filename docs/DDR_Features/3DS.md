
## Introduction

Traditionally we have had DDR with multiple dies inside the package. This allowed to have higher density DRAM devices. These devices would be Dual Die Package or Quad Die Package, we would have die sitting next to each other but not stacked on each other. But starting with DDR4, we have 3DS which is 3 Dimensional Stacking of die in single package. 3DS devices uses TSV (Through Silicon Vias) to make connection between the dies.

**Side View of the QDP vs 4H 3DS** (Source: JEDEC Presentation)

![](../images/3ds/qdpvs4h.drawio)

**Top View of the DDP vs 2H 3DS** (Source: SK Hynix Newsroom )

![](../images/3ds/skhynix3ds)

## Configuration

DDR supports multiple package options:

* Monolithic Single Die Package (SPD)
* Standard Dual Die Package (DDP)
* Standard Quad Die Package (QDP)
* 3DS Package (Three Dimensional Stacks) - on DDR4 and DDR5
    * 2-High (2 Logical Ranks with single signal loads)
    * 4-High (4 Logical Ranks with single signal loads)
    * 8-High (8 Logical Ranks with single signal loads)

To support these many logical ranks, in traditional system we would need that many Chip Select signals i.e. for 8 Ranks we would need 8 indivdual Chip Selects pins, which is very costly. So unlike previous DDR generations, 3DS memory architecture was introduced on DDR4. DDR4/DDR5 uses new pins called CID i.e. Chip ID which is used along with Single Chip Select per package to select the logical rank within the package.

### 2-High Configuration

|  Ranks  |      C2     |   C1    |      C0      | Chip Select (CS_n). |
| :--------: |:-------------:| :---------:| :---------: | :---------: | 
| Logical Rank 0 | Not Used| Not Used | 0 | Low |
| Logical Rank 1 | Not Used| Not Used | 1 | Low |

### 4-High Configuration

|  Ranks  |      C2     |   C1    |      C0      | Chip Select (CS_n). |
| :--------: |:-------------:| :---------:| :---------: | :---------: | 
| Logical Rank 0 | Not Used| 0 | 0 | Low |
| Logical Rank 1 | Not Used| 0 | 1 | Low |
| Logical Rank 2 | Not Used| 1 | 0 | Low |
| Logical Rank 3 | Not Used| 1 | 1 | Low |

### 8-High Configuration

|  Ranks  |      C2     |   C1    |      C0      | Chip Select (CS_n). |
| :--------: |:-------------:| :---------:| :---------: | :---------: | 
| Logical Rank 0 | 0| 0 | 0 | Low |
| Logical Rank 1 | 0| 0 | 1 | Low |
| Logical Rank 2 | 0| 1 | 0 | Low |
| Logical Rank 3 | 0| 1 | 1 | Low |
| Logical Rank 4 | 1| 0 | 0 | Low |
| Logical Rank 5 | 1| 0 | 1 | Low |
| Logical Rank 6 | 1| 1 | 0 | Low |
| Logical Rank 7 | 1| 1 | 1 | Low |

## 3DS Architecture

3DS architecture introduces concept of Master-Slave dies. In 3DS, there is only one die which act as a Master die, interacting with external world. Other dies are slave dies, which interact with Master die. In 3DS memory, only bottom most die is connected externally to Memory Controller + PHY. The remanining dies are interconnected through many TSVs internally providing Input/Output load isolation.

![](../images/3ds/3dsarchitecture.drawio)

From this you can see that in Traditional QDP Package, external connection is common to all the dies within the package. However, on 3DS package, only Master die is connected to Memory Controller + PHY. This means that on Traditional QDP package, we have 4x the loads as compared to 3DS package, which is only seen as 1 die external load to Memory controller + PHY.

![](../images/3ds/packaging.drawio)


## 3DS Pros and Cons

|  Pro  |      Cons     |   
| :--------: |:-------------:| 
| * Operate at faster speeds and lower pj/bit. <br> * Lower load | * COmplexity in designing|
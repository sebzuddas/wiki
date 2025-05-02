
There are multiple python libraries for conducting [[System Identification]]. 

|             Library Name              |       Installation       |                        Link                        | Videos                                                                              |
| :-----------------------------------: | :----------------------: | :------------------------------------------------: | ----------------------------------------------------------------------------------- |
|                 SIPPY                 |                          |       https://github.com/CPCLAB-UNIPI/SIPPY        |                                                                                     |
| [SysidentPy](https://sysidentpy.org/) | `pip install sysidentpy` |      https://github.com/wilsonrljr/sysidentpy      |                                                                                     |
|                PySINDy                |  `pip install pysindy`   |       https://github.com/dynamicslab/pysindy       | https://www.youtube.com/watch?v=SfIJiuJ38W0&list=PLN90bHJU-JLoOfEk0KyBs2qLTV7OkMZ25 |
|               PyDSTool                |                          | https://pydstool.github.io/PyDSTool/FrontPage.html |                                                                                     |

---
# SysIdentPy



---
# PySINDy


[PySINDy and MPC](https://arxiv.org/pdf/2108.13404)
There is only one example of PySINDy being used in an [[Model Predictive Control (MPC)|MPC]] context, which is more social systems oriented. 

Repo where they have used PySINDy for modelling process dynamics:
https://github.com/ihmstefanini/dynamic-modeling-pysindy/blob/main/src/fit.py

It seems that PySINDy hasn't been used for process dynamics much, which is what we're interested in. 

#### Questions about PySINDy
1. How do you account for a systems' reaction to unputs $u$? Do we need to account for these?
2. Do you need to give it basis functions? Candidate models? What do you need to give it beyond state data?
3. Sample rates & noise seem to be something to be aware of when working with this. Are there ways to reduce? Could use an SNR value, which could be a sensor attribute.
4. Can you add a column of one particular value to represent a constant?



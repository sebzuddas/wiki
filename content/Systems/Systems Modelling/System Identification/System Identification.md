The process of identifying [[Ordinary Differential Equations (ODEs)]] models from data. Used to then inform [[State-space Control]] or [[Model Predictive Control (MPC)]] schemas. All of these require a [[Dynamic Systems]] model, and when you can't model from first principles, System Identification provides a data-driven approach based on input and output data. 

https://www.diva-portal.org/smash/get/diva2:277019/FULLTEXT02
https://www.diva-portal.org/smash/get/diva2:315864/FULLTEXT02

# The Basic Ideas
https://www.diva-portal.org/smash/get/diva2:315864/FULLTEXT02


## System Identification for Multi-rate Systems 
Multi-rate Systems are systems where the sample rate for different components are sampled at different sampling frequencies, ie $f_{s_1} \neq f_{s_2}$. This [paper](https://www.mic-journal.no/PDF/2014/MIC-2014-3-1.pdf) demonstrates a method to do system identification for an aluminium smelter, which is known for being a multi-rate system. 

# Parametric Identification

## AR

## ARX

## ARMA

## ARMAX

## NARMAX
Non-linear Auto Regressive Moving Average with Exogenous Input. Attaining a NARMAX model requires the following steps:


- Dynamical tests and collecting data;
- Choice of mathematical representation;
- Detection of the model structure;
- Estimation of parameters;
- Validation;
- Analysis of the model


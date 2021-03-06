# Homomorphically Encrypted Time Series Analysis  (HETSA)  

## Statement

The aim of this project is to develop a framework for analysing homomorphically encrypted time series (HETSA). Namely, we will implement a set of building blocks for time series analysis and showcase them on the following applications: First, to demonstrate the capabilities of real-time processing, we will implement a set of trading strategies that make trading decisions on encrypted market data (Done). Second, to demonstrate the developed signal analysis tools, we will analyse encrypted heart rate variability of patients with diabetes that consequently will support clinical judgement and decision-making (TODO). Third, to show the versatility, we will implement a set of general time-series analysis tools in homomorphic encryption context (DOING). Furthermore, we plan to develop a bootstrapping technique that provides the means for real time data processing. We believe that focusing on these areas, where issues surrounding privacy play a crucial role, will not only showcase the potential of the proposed framework, but also ignite further research and development.  

The trading strategy on encrypted market data has been implemented and accepted in the form of a short [paper](https://www.scitepress.org/Documents/2020/98357/) by the 17th International Conference on Security and Cryptography (SECRYPT). The source codes are located in the FinancialApplications folder.

The goal of this project includes implementing a variety of time series analysing algorithms and a wrapper based on several homomorphic encryption libraries.

![The structure of the toolbox](toolbox-structure.png)

## Current problems and limitations

The idea of homomorphic encryption was proposed in the 1970s as a solution to privacy-preserving computation. Modern cryptosystems are capable of performing arithmetic operations on ciphertexts, facilitating the implementation of various machine learning algorithms in the homomorphic encryption context. This is an active area of research, and therefore a multitude of encryption schemes have been proposed and implemented as open-source libraries. In particular, [BFV scheme](https://eprint.iacr.org/2012/144.pdf) only allows arithmetic operations on integers while [CKKS scheme](https://eprint.iacr.org/2016/421.pdf) supports approximate number arithmetic operations. [Microsoft SEAL Library](https://github.com/Microsoft/SEAL) supports both BFV and CKKS scheme but [HEAAN Library](https://github.com/snucrypto/HEAAN) only implements CKKS scheme.  

Nevertheless, existing libraries have a number of limitations: First, an array of open source libraries that implement homomorphic encryption schemes are available to choose from, and thereby, complicate the decision to choose the most relevant one. Second, these libraries are relatively low-level and implement elementary operations such as addition and multiplication, hence, making the implementation of data analysis algorithms arduous. Third, scalars, vectors, and matrices are mapped to sets of coefficients that make the union and intersection operations over encrypted datasets either impossible or tedious, thus making the process of real-time time series analysis cumbersome. Last but not least, as the state-of-art [bootstrapping method in CKKS scheme](https://eprint.iacr.org/2018/153.pdf) is inefficient and impractical, at the moment the multiplication level of our algorithms is restricted by the parameters of the CKKS scheme and thus it is difficult to implement recursive algorithms.  

## Team

Haotian Weng, Artem Lenskiy

## Applications

To demonstrate the capabilities of the proposed framework we present the following use cases.

```bash
GeneralToolbox
    └── Linear Model: EMA - DONE, ARMA - DOING
FinancialApplications - DONE
    ├── macd-HEAAN
    │
    ├── macd-Plaintext
    │
    └── macd-SEAL
MedicalApplications - TODO
```

### General Toolbox

#### Autoregressive Moving Average Model

[ARMA model](https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model), which is a popular time-series analysis tool, is the combination of Autoregressive (AR) model and Moving Average (MA) model. AR model is a linear regression model which predicts future values based on the history data and works well when there is correlation between the values in the time-series. MA model generates a value linearly dependent on the current and past values, which indicates the trend or momentum of the time-series data. Combining the advantages of AR and MA model, ARMA model usually requires fewer parameters than AR or MA model alone.  With the implementation of ARMA model in homomorphic encryption context, time-series analysis on various encrypted data becomes possible, which is the reason why it is a general-purpose tool for HETSA.

#### Exponential Moving Average

[EMA](https://en.wikipedia.org/wiki/Moving_average) is a first-order infinite impulse response filter that is widely used in finanial area. It can be applied to analysing stock markets and is utilised in a variety of trading strategies.

<!-- #### Echo State Network

Echo State Network (ESN) is one type of Recurrent Neural Networks with sparsely connected reservior as well as untrainble input-reservoir and reservoir-reservoir weights. As ESN requires less training, it is relatively more efficient than traditional neural networks. In addition, it can avoid gradient exploding or vanishing problem to some extent so that it usually performs better on chaotic time-series data. We will implement ESN on encrypted financial data in the future.   -->

### Financial Applications - Moving Average Convergence Divergence

Algorithmic trading has proliferated the area of quantitative finance for already a number of decades. The decisions are made in an automatic manner using the data provided by brokerage firms and exchanges. There is an emerging intermediate layer of financial players that is placed in between a broker and algorithmic traders. The role of these players is to aggregate market decisions from the algorithmic traders and send a final market order to a broker. In return the quants receive incentives proportional to the correctness of their predictions. In such a setup, the intermediate player - an aggregator does not provide the market data in plaintext but encrypts it. Encrypting market data prevents quants from trading on their own, as well as keeps expensive financial data private. In this use case we implement a [MACD](https://en.wikipedia.org/wiki/MACD)-based trend-following strategy with homomorphic encryption methods.  

### Medical Applications - Diabetes diagnosis on encrypted heart rate variability - TODO

In this application, the HETSA library is employed to detect diabetes through the analysis of the encrypted heart rate variability (HRV). HRV is usually measured by a wearable device (e.g. smartwatch) and is transmitted over the Internet in an encrypted form to a remote server for further processing. In order to make a diagnosis, the HETSA implements spectral analysis and linear classifiers.  

## Getting started  

### Dependencies

[Microsoft SEAL Library](https://github.com/Microsoft/SEAL)  
[HEAAN Library](https://github.com/snucrypto/HEAAN)  
[NTL](https://www.shoup.net/ntl/doc/tour-unix.html)  
cmake  
gcc/g++

NTL has to be installed first as HEAAN uses it.

Microsoft SEAL Library needs to be installed beforehand from the above link.  

HEAAN Library is included but needs to be built.  

```bash
cd HEAAN/lib
make all
```

Please note that it may take dozens of minutes to run one program as the state-of-the-art homomorphic encryption libraries are still extremely inefficient. Generally speaking, SEAL outpasses HEAAN in terms of efficiency.  

### General Toolbox Implementation

#### EMA model with HEAAN

```bash
cd GeneralToolbox/EMA-HEAAN
make
./EMA
```  

Run `ema.ipynb` in Jupyter Notebook to generate EMA signals based on plaintext data and compare the outputs of the plaintext and ciphertext approaches.  

#### ARMA in Plaintext

Run the test cases in ARMA-Plaintext/TestsAndDemos in MATLAB.  

#### ARMA model with HEAAN

DOING  

### Financial Application

Please note that the minimum RAM required is 32GB as ciphertexts are memory-consuming.  

#### MACD with SEAL  

```bash
cd FinancialApplications/macd-SEAL
cmake .
make
./macd
```

#### MACD with HEAAN

```bash
cd FinancialApplications/macd-HEAAN
make
./MACD
```

#### MACD in Plaintext

Run `macd.ipynb` in Jupyter Notebook to generate MACD signals and trading decisions based on plaintext data.  

#### Visualisation

Run `output-comparison.ipynb` in Jupyter Notebook to visualise results.  

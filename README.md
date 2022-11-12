
# Securely Realizing Output Privacy in MPC using Differential Privacy
### Differential Privacy in MPC

This is an extension of the [MOTION framework](https://github.com/encryptogroup/MOTION) that supports secure multi-party computation (MPC) with semi-honest and full-threshold security. We evaluate the potential of applying differential privacy (DP) in MPC by implementing various noise generation methods and following *differentially private mechanisms*:

| Algorithm | Noise Type  | Reference|
|--|--|--|
|  Laplace Mechanism*|  Laplace noise|[PrivaDA](https://dl.acm.org/doi/10.1145/2664243.2664263)|
|  Discrete Laplace Mechanism*|   Discrete Laplace noise|[PrivaDA](https://dl.acm.org/doi/10.1145/2664243.2664263)|
|  Gaussian Mechanism*|   Gaussian noise|[CrypTen](https://arxiv.org/abs/2109.00984)|
|  Laplace Mechanism|   Discrete Laplace noise|[Google DP Team](https://github.com/google/differential-privacy/blob/main/common_docs/Secure_Noise_Generation.pdf)|
|  Gaussian Mechanism|   Binomial noise|[Google DP Team](https://github.com/google/differential-privacy/blob/main/common_docs/Secure_Noise_Generation.pdf)|
|  Discrete Laplace Mechanism|   Discrete Laplace noise|[Canonee et al.](https://arxiv.org/abs/2004.00010)|
| Discrete Gaussian Mechanism|   Discrete Gaussian noise|[Canonee et al.](https://arxiv.org/abs/2004.00010)|

*Note: Algorithms marked with * suffer from the floating-point attacks discussed below.* 

Furthermore, we consider the **floating-point attacks** (that can break the DP security guarantee, see [paper1](https://dl.acm.org/doi/abs/10.1145/2382196.2382264?casa_token=HQ-TlyeCspEAAAAA:zXxt2vEdDDfWNpfLhsT-DKNc58NA_tm693z-AIaroBAZmUosS-oceIPS9s2RY64QlSVT2mBk5J_5), [paper2](https://arxiv.org/abs/2112.05307))  and implement differentially private mechanisms using *secure* noise generation methods in MPC.

---
### Arithmetic Operations in MPC

We extend [MOTION](https://github.com/encryptogroup/MOTION) to support *signed integer (INT)*, *fixed-point (FX)* and *floating-point (FL)* arithmetic in following MPC protocols:
 - Boolean Goldreich-Micali-Wigderson (BGMW) 
 - Beaver-Micali-Rogaway (BMR)
 - Arithmetic Goldreich-Micali-Wigderson (AGMW) 

Specifically, we support following arithmetic operations for real number *x* and *y*: 
|Number Representation|Arithmetic Operation | MPC Protocol |
|--|--|--|
|INT, FX, FL| Addition (+), Subtraction (-), Division (/), Multiplication (*), Comparison (<, >, ==), Negation (-), Absolute Value (\|x\|) |BGMW, BMR, AGMW  |
|INT| Modulo Reduction (%) |BGMW, BMR, AGMW  |
|FX, FL| $2^{x}$, $e^{x}$, $\log_2 x$, $\ln x$, $\sqrt x$, Rounding (floor, ceil, round-toward-zero), Triangular functions ( $\sin x$, $\cos x$) |BGMW, BMR, AGMW  |
|INT, FX, FL| Conversion between different number representations |BGMW, BMR, AGMW  |

The differentially private mechanisms are primarily implemented in BGMW because it is faster than BMR and AGMW regarding the total (offline + online) run-times.  
The complete benchmark results can be found at [benchmark_results](https://github.com/liangzhao-darmstadt/Securely-Realizing-Output-Privacy-in-MPC-using-Differential-Privacy/tree/dev/src/examples/liangzhao_benchmark_result/benchmark_result_server_20220630).

## Build Instructions
*Please see [MOTION](https://github.com/encryptogroup/MOTION) for a detail description.* 
This software was developed and tested in the following environment:
* `Ubuntu 20.04` 
*  `g++` (version >=10)
*  `make`
*  `cmake`
* [`boost`](https://www.boost.org/) (version >=1.75.0)
*  `OpenMP`
* [`OpenSSL`](https://www.openssl.org/) (version >=1.1.0)

1. Clone the MOTION git repository by running:
```
git clone https://github.com/liangzhao-darmstadt/Securely-Realizing-Output-Privacy-in-MPC-using-Differential-Privacy.git
```
2. Enter the Framework directory: `cd Securely-Realizing-Output-Privacy-in-MPC-using-Differential-Privacy/`
3. Create and enter the build directory: `mkdir build && cd build`
4. Use CMake configure the build: ```cmake ..```
5. Option: Build MOTION executables and test cases ```cmake .. -DMOTION_BUILD_EXE=On```, ```cmake .. -DMOTION_BUILD_TESTS=On```. Choose build type: ```Debug``` using ```cmake .. -DCMAKE_BUILD_TYPE=Debug```, ```Release``` using ```cmake .. -DCMAKE_BUILD_TYPE=Release```.
7. Call `make` in the build directory.
Optionally, add `-j $number_of_parallel_jobs` to `make` for faster compilation.

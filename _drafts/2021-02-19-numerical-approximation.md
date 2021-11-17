---
title: 数值估计
date: 2021-01-05 09:42:35
tags: [vim,tips,linux]
categories: [vim]
---

Before embarking on crafting a custom implementation, it seems advisable to check whether the CDF of the standard normal distribution is supported as a built-in function in the programming environment of your choice. For example, MATLAB offers a function normcdf, as does CUDA.

If no implementation of normcdf is available, the next thing to check is whether the programming environment offers an implementation of the complementary error function, i.e. erfc. This exists as ERFC in Fortran 2008, and as erfc() in C99 and C++11, for example. The CDF of the standard normal distribution is related to erfc

as follows:

𝑃(𝑥)=12erfc(−𝑥12‾‾√)

The reason for using erfc
instead of erf is to avoid subtractive cancellation that leads to inaccuracy in the tails. Note that for 𝑥<0, there is error magnification, that is, the relative error incurred in scaling the argument to erfc is magnified. Computing 𝑥12‾‾√ should be performed as accurately as possible, and if the final result 𝑃(𝑥)

is subject to tight ulp error bounds, additional compensation may be needed. A technique for computing such a product with maximum accuracy with the help of a fused-multiply add (FMA) operation is presented in this paper:

Nicolas Brisebarre and Jean-Michel Muller, "Correctly Rounded Multiplication by Arbitrary Precision Constants." IEEE Transactions on Computers, Vol. 57, No. 2, February 2008, pp. 165-174 (draft online)

The FMA operation, which computes 𝑎𝑏+𝑐

with a single rounding at the end is available as the standard math function fma() in C99 and C++11.

If a given computational environment does not provide a built-in way to compute erfc

, you might want to look into using or porting robust code for the computation of error functions by W. J. Cody which can be found on Netlib. That Fortran code is based on the following paper:

W. J. Cody, "Rational Chebyshev approximations for the error function." Mathematics of Computation, Vol 23., No. 107 (1969), pp. 631-637. (online)

If Cody's code is not suitable for your work (e.g. due to licensing issues), a custom implementation of erfc

could be created based on the following paper:

M. M. Shepherd and J. G. Laframboise, "Chebyshev approximation of (1+2𝑥)exp(𝑥2)erfc𝑥
in 0⩽𝑥<∞

." Mathematics of Computation, Vol. 36, No. 153 (Jan., 1981), pp. 249-253 (online)

I used the algorithm from this paper as the basis for the implementation of erfc() for a shipping parallel computation platform, but used a freshly generated polynomial minimax approximation rather than the Chebyshev approximation from the paper. Tools like Maple or Mathematica offer functionality for generating such minimax approximations, or you could generate your own using the Remez algorithm.

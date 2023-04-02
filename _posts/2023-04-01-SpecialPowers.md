---
layout: post
title: Special Powers
---

****** NOTE: The markdown rendering of this file is NOT correct. *****
See pdf version at https://web.ncf.ca/nashjc/BlogCopies/SpecialPowers.pdf


### John C. Nash   2023-4-1

In a recent blog post, ([“Interest-ing Calculations"](https://nashjc.github.io/Interest-ingCalculations/))
, I mentioned that Canadian mortgages often need the
calculation of an **effective interest rate** that is equivalent to an
annual or semi-annual rate. For monthly payments of Canadian mortgages,
for instance, the usual (fractional) rate *r*<sub>*e*</sub> used to
compute the monthly payment for a nominal annual percentage rate *R* is

*r*<sub>*e*</sub> = (1+*R*/200)<sup>(1/6)</sup> − 1

Call this Equation 1.

As a percentage, the monthly rate would be 100 \* *r*<sub>*e*</sub>.

The fractional rate is used in the updating formula, a recurrence
relationship that provides the new balance *A*<sub>*i*</sub> at the end
of period (month) *i* from the previous period balance
*A*<sub>*i* − 1</sub> when a payment of *p*<sub>*i*</sub> is made. (We
may have to specify when the payment is made as part of the contract.)

*A*<sub>*i*</sub> = *rounded*(*A*<sub>*i* − 1</sub>\*(1+*r*<sub>*e*</sub>)−*p*<sub>*i*</sub>)

where *rounded*() should be a function returning the balance
to the nearest cent or other smallest unit of currency. We use
**should** because some institutions may use different rounding rules.
Usually we want the payment to be the same across periods, 
i.e., *p*<sub>*i*</sub> = *p*, but by allowing it to be variable, we can include balloon
payments or other conditions. We could also consider variation in the
effective rate, since the same formula applies to variable rate loans.

## Computing the effective rate

In most modern computing languages, we can find the effective rate by
applying the formula for *r*<sub>*e*</sub> above. This relies on the
computing environment having a good approximation for the power
function. *x*<sup>*y*</sup> = *pow*(*x*,*y*). Such approximations
are a difficult and detailed science. Some references are given at the
end. (Muller (2006), William J. Cody, Jr. and William Waite (1980),
Crenshaw (2000)). Fortunately, most modern computing systems, including
R, do a good job.

In our present situation, we have a quite special case.

First, we are (usually) dealing with a value of *R* that is in the range
0 to 25. Even 25% annual interest has been extreme, though in the early
1980s I had to calculate a mortgage table for 21%. This means that we
are trying to compute, for example, 1.115<sup>(1/6)</sup>. This means
there is the possibility of using **table lookup**, possibly with
interpolation between values on the table. We won’t explore that further
here, but it can be a very useful technique when the range of inputs is
restricted.

Second, we generally have exponents that are reciprocals of integers. In
the example we want the 1/6 power. This suggests that we can rewrite
Equation 1 as

(1+*r*<sub>*e*</sub>)<sup>6</sup> = (1+*R*/200)

or

*f*(*r*<sub>*e*</sub>) = (1+*r*<sub>*e*</sub>)<sup>6</sup> − (1+*R*/200) = 0

which defines a **root-finding** problem. R has the function `uniroot()`
in the `stats` package of the distributed code that solves this problem.
There is also `unirootR()` in the extended-precision package Rmpfr
(Maechler (2023)).

Third, we are computing the power of a binomial, and indeed, a
particular form of a binomial. Writing *x* = *R*/200, and assuming the
exponent *a* is non-integer, we have the Taylor-McLaurin expansion

 (1 + *x*)<sup>a</sup> 
   = sum<sub>{i=1}</sub><sup>infinity</sup> {F<sub>i</sub>} 
   = 1 + *x*\**a* + *x*<sup>2</sup>\* *a* \* (*a*-1)/2 + ...   

where *F*<sub>*0*</sub> = 1 and

*F*<sub>*i* + 1</sub> = *x* \* *F*<sub>*i*</sub> \* (*a*−*i*)/*i* .

This binomial expansion approach has the added nicety that we can drop
the 1 from the summation, avoiding a cancellation of digits when the
resulting *r*<sub>*e*</sub> is small.

## Prerequisite integer powers function

For the root-finding method, we need to be able to compute integer
powers of real (i.e. floating-point) numbers. Clearly one algorithm is

    naipwr <- function(a, n){ # naive power algorithm a^n, n integer
       val <- 1
       for (i in 1:n) { val <- val * a}
       val
    }

This has (for this algorithm), `n` multiplications, though we could also
do

    naipwr1 <- function(a, n){  # naive power algorithm a^n, n integer
       val <- a
       if (n > 1){
       for (i in 1:(n-1)) { val <- val * a}
       }
       val
    }

Neither of these codes have checks on the value of `n` that would be
needed for a general-purpose function. However, such checks are
sometimes costly of time. If we can be sure inappropriate inputs are
never provided, we can omit the checks and outputs, and these have been
suppressed by commenting them out.

It is possible to do fewer multiplications, and various workers have
explored this. (See, for example,
<https://ece.uwaterloo.ca/~dwharder/Algorithmic_thinking/Integer_powers/>
or
<https://www.geeksforgeeks.org/write-a-c-program-to-calculate-powxn/>).

An R implementation of a “fast” power function is as below. Note that
this is still not “optimal” in that in many cases it uses more than the
absolute minimum number of multiplications.

    fastpow <- function(x, n){
      #  if (! is.integer(n)) stop("MUST have integer power")
      res <- 1.0
      tn <- n # temporary n
      tx <- x
      while (tn > 0){
        #    cat("tn, tx:",tn," ",tx)
        #    readline("  OK?")
        if ((tn %% 2) != 0){
          res <- res * tx
          tn <- tn - 1
        }
        tn <- tn/2
        tx <- tx*tx
      }
      res
    }

Let us see if there is much difference in the timings and outputs of
these codes.

    a = 1.1
    n = 57
    pR <- a^n
    pf <- fastpow(a, n)
    pn <- naipwr(a,n)
    pn1 <- naipwr1(a,n)
    dfR <- pf-pR
    dnR <- pn-pR
    dn1R <- pn1-pR
    dfn <- pf-pn
    dfn1 <- pf-pn1
    dnn1 <- pn-pn1
    cat("pf = ",pf,"\n")

    ## pf =  228.7616

    cat("Differences: pf from pR, pn, pn1:", dfR, dfn, dfn1,"\n")

    ## Differences: pf from pR, pn, pn1: -1.421085e-13 0 0

    cat("Differences: pn from pR, pn1:", dnR, dnn1,"\n")

    ## Differences: pn from pR, pn1: -1.421085e-13 0

    cat("Differences: pn1 from pR:", dn1R,"\n")

    ## Differences: pn1 from pR: -1.421085e-13

    cat("Timings\n")

    ## Timings

    library(microbenchmark)
    microbenchmark(a^n)

    ## Unit: nanoseconds
    ##  expr min  lq   mean median  uq  max neval
    ##   a^n 120 130 182.07    130 131 5170   100

    microbenchmark(naipwr(a,n))

    ## Unit: microseconds
    ##          expr   min    lq    mean median     uq    max neval
    ##  naipwr(a, n) 1.783 1.884 2.04996  1.923 2.0585 10.289   100

    microbenchmark(naipwr1(a,n))

    ## Unit: microseconds
    ##           expr   min    lq    mean median     uq    max neval
    ##  naipwr1(a, n) 1.813 1.894 2.13817 1.9285 2.0835 12.774   100

    microbenchmark(fastpow(a,n))

    ## Unit: microseconds
    ##           expr   min    lq    mean median    uq    max neval
    ##  fastpow(a, n) 1.583 1.613 2.52578 2.0385 2.981 16.802   100

This little trial suggests - the internal functions (which are
pre-compiled in C and not using the R interpreter) do very well on speed
and are almost certainly accurate enough for our needs - the `fastpow()`
function is marginally faster than the naive methods, as we expect

We also tried compiling the codes using the `compiler` package (R Core
Team (2023)), but the results were essentially the same.

# Effective rate via rootfinding

Let us use `uniroot` to find the effective rate. Here are a pair of
small functions to do this.

    mpow <- function(x, m=NA, rate=NA){ # Note rate is passed from environment, as is m
      fastpow((1+x), m)-1-rate
    }
    mroot <- function(m, rate, tol=1e-16){# use rootfinding to get fractional rate for 1/m period
      xx <- uniroot(f=mpow, lower=0, upper=1+2*rate, tol=tol, m=m, rate=rate)
      res<-xx$root
      attr(res, "fval") <- xx$f.root
      attr(res, "iter") <- xx$iter
      attr(res, "prec") <- xx$estim.prec
      res
    }
    mrootdisp <- function(vmroot){
       cat("root =",vmroot," fval=",attr(vmroot,"fval")," iter=",attr(vmroot,"iter"),
            "  prec=",attr(vmroot,"prec"),"\n")
       invisible(NULL)
    }

Some notes:

-   `uniroot` wants a function of 1 argument for which it will find the
    root, but we have to provide the `rate` (e.g., for 6 months, as a
    fraction) and the number of periods `n` (e.g., 6). We put these
    arguments in the extra or “dot arguments” at the end of the call.
    This is one aspect of uniroot that I find awkward.

-   I have noticed that a tight tolerance `tol` is needed. This is much
    smaller than the default. Otherwise the results are not as precise
    as they can be. The default is `tol = .Machine$double.eps^0.25` or
    0.0001220703 = 1.220703e-4

-   we return the number of iterations and the final function value.
    This value is the evaluated function of which we want the root. It
    should be very close to zero.

Let us test this.

    m <- 6
    rate <- 0.1
    mrootdisp(mroot(m, rate))

    ## root = 0.01601187  fval= 3.053113e-16  iter= 12   prec= 5.551115e-17

    mrootdisp(mroot(m, rate, tol=1e-4))

    ## root = 0.01602186  fval= 6.489569e-05  iter= 9   prec= 5e-05

    mrootdisp(mroot(m, rate, tol=1e-8))

    ## root = 0.01601187  fval= -6.773609e-12  iter= 11   prec= 5e-09

    mrootdisp(mroot(m, rate, tol=1e-12))

    ## root = 0.01601187  fval= 3.053113e-16  iter= 12   prec= 5.000063e-13

    mrootdisp(mroot(m, rate, tol=1e-16))

    ## root = 0.01601187  fval= 3.053113e-16  iter= 12   prec= 5.551115e-17

    mrootdisp(mroot(m, rate, tol=1e-20))

    ## root = 0.01601187  fval= 3.053113e-16  iter= 15   prec= 1.040834e-17

## Effective rate via binomial expansion

We implement the binomial expansion via a recurrence relation and sum
the expansion until there is no change. The code allows us to save the
sequence of terms, which could be summed “backwards” for best accuracy
when the terms are diminishing.

    # binex.R -- binomial expansion of (1+x)^a (a likely fractional)
    binex <- function(x, a, nt=100){
      # load nt terms starting with i=1 (implicit 1 at i=0)
       trm <- rep(0,nt)
       T <- 1
       S <- 0
       Slast <- -1
       i <- 1
       while( (i <= 100) && (S != Slast) ){
          ii <- i-1
          T <- T*(a - ii)*x/i
          trm[i]<-T
          Slast <- S
          S <- S + T
          i <- i + 1
       }
       res <- list(trm=trm, S = S, itn = i)
       res
    }

    # test
    m <- 6
    a <- 1/m
    x <- 0.1 # rate above
    res <- binex(x, a)
    r <- sum(res$trm)
    rx <- (1+x)^a - 1
    cat("a=",a,"  x=",x,"  r=",r,"  rx=",rx,"  S=",res$S,"\n")

    ## a= 0.1666667   x= 0.1   r= 0.01601187   rx= 0.01601187   S= 0.01601187

## Conclusion

We have a choice of methods to compute the effective per-period rate
when there are conditions such as those for Canadian mortgages where the
monthly rate must be “equivalent to” the semiannual one. It turns out
that all these approaches are satisfactory for R.

## References

Crenshaw, Jack. 2000. *Math Toolkit for Real-Time Programming (1st
Ed.)*. CRC Press. <https://doi.org/10.1201/9781482296747>.

Maechler, Martin. 2023. *Rmpfr: R MPFR - Multiple Precision
Floating-Point Reliable*. <https://CRAN.R-project.org/package=Rmpfr>.

Muller, Jean-Michel. 2006. *Elementary Functions - Algorithms and
Implementation (2. Ed.).* Birkhäuser.

R Core Team. 2023. *R: A Language and Environment for Statistical
Computing*. Vienna, Austria: R Foundation for Statistical Computing.
<https://www.R-project.org/>.

William J. Cody, Jr. and William Waite. 1980. *<span
class="nocase">Software Manual for the Elementary Functions</span>*.
Prentice Hall.

# Mathematical Functions

Though the central data structure in Owl is `ndarray`, we provide support for scalar math functions. 

## Basic Functions

Binary functions:

```
val add : float -> float -> float
(** ``add x y`` returns :math:`x + y`. *)

val sub : float -> float -> float
(** ``sub x y`` returns :math:`x - y`. *)

val mul : float -> float -> float
(** ``mul x y`` returns :math:`x * y`. *)

val div : float -> float -> float
(** ``div x y`` returns :math:`x / y`. *)

val fmod : float -> float -> float
(** ``fmod x y`` returns :math:`x % y`. *)

val atan2 : float -> float -> float
(** ``atan2 y x`` returns :math:`\arctan(y/x)`, accounting for the sign of the arguments;
 this is the angle to the vector :math:`(x, y)` counting from the x-axis. *)

```

Unary functions with signature `val f : float -> float`

------------  --------------------------------------------
Function      Explanation  
------------  --------------------------------------------
`abs`         `|x|`

`neg`         `-x`

`reci`        `1/x`

`floor`       the largest integer that is smaller than `x`
------------  --------------------------------------------

```
al ceil : float -> float
(** ``ceil x`` returns the smallest integer :math:`\geq x`. *)

val round : float -> float
(** ``round x`` rounds, towards the bigger integer when on the fence. *)

val trunc : float -> float
(** ``trunc x`` integer part. *)

val sqr : float -> float
(** ``sqr x`` square. *)

val sqrt : float -> float
(** ``sqrt x`` square root. *)

val pow : float -> float -> float
(** ``pow x y`` returns :math:`x^y`. *)

val exp : float -> float
(** ``exp x`` exponential. *)

val exp2 : float -> float
(** ``exp2 x`` exponential. *)

val exp10 : float -> float
(** ``exp10 x`` exponential. *)

val expm1 : float -> float
(** ``expm1 x`` returns :math:`\exp(x) - 1` but more accurate for :math:`x \sim 0`. *)

val log : float -> float
(** ``log x`` natural logarithm *)

val log2 : float -> float
(** ``log2 x`` base-2 logarithm. *)

val log10 : float -> float
(** ``log10 x`` base-10 logarithm. *)

val logn : float -> float -> float
(** ``logn x`` base-n logarithm. *)

val log1p : float -> float
(** ``log1p x`` returns :math:`\log (x + 1)` but more accurate for :math:`x \sim 0`.
 Inverse of ``expm1``. *)

val logabs : float -> float
(** ``logabs x`` returns :math:`\log(|x|)`. *)

val sigmoid : float -> float
(** ``sigmoid x`` returns the logistic sigmoid function
:math:`1 / (1 + \exp(-x))`. *)

val signum : float -> float
(** ``signum x`` returns the sign of :math:`x`: -1, 0 or 1. *)

val softsign : float -> float
(** Smoothed sign function. *)

val softplus : float -> float
(** ``softplus x`` returns :math:`\log(1 + \exp(x))`. *)

val relu : float -> float
(** ``relu x`` returns :math:`\max(0, x)`. *)

val sin : float -> float
(** ``sin x`` returns :math:`\sin(x)`. *)

val cos : float -> float
(** ``cos x`` returns :math:`\cos(x)`. *)

val tan : float -> float
(** ``tan x`` returns :math:`\tan(x)`. *)

val cot : float -> float
(** ``cot x`` returns :math:`1/\tan(x)`. *)

val sec : float -> float
(** ``sec x`` returns :math:`1/\cos(x)`. *)

val csc : float -> float
(** ``csc x`` returns :math:`1/\sin(x)`. *)

val asin : float -> float
(** ``asin x`` returns :math:`\arcsin(x)`. *)

val acos : float -> float
(** ``acos x`` returns :math:`\arccos(x)`. *)

val atan : float -> float
(** ``atan x`` returns :math:`\arctan(x)`. *)

val acot : float -> float
(** Inverse function of ``cot``. *)

val asec : float -> float
(** Inverse function of ``sec``. *)

val acsc : float -> float
(** Inverse function of ``csc``. *)

val sinh : float -> float
(** Returns :math:`\sinh(x)`. *)

val cosh : float -> float
(** ``cosh x`` returns :math:`\cosh(x)`. *)

val tanh : float -> float
(** ``tanh x`` returns :math:`\tanh(x)`. *)

val coth : float -> float
(** ``coth x`` returns :math:`\coth(x)`. *)

val sech : float -> float
(** ``sech x`` returns :math:`1/\cosh(x)`. *)

val csch : float -> float
(** ``csch x`` returns :math:`1/\sinh(x)`. *)

val asinh : float -> float
(** Inverse function of ``sinh``. *)

val acosh : float -> float
(** Inverse function of ``cosh``. *)

val atanh : float -> float
(** Inverse function of ``tanh``. *)

val acoth : float -> float
(** Inverse function of ``coth``. *)

val asech : float -> float
(** Inverse function of ``sech``. *)

val acsch : float -> float
(** Inverse function of ``csch``. *)

val sinc : float -> float
(** ``sinc x`` returns :math:`\sin(x)/x` and :math:`1` for :math:`x=0`. *)

val logsinh : float -> float
(** ``logsinh x`` returns :math:`\log(\sinh(x))` but handles large :math:`|x|`. *)

val logcosh : float -> float
(** ``logcosh x`` returns :math:`\log(\cosh(x))` but handles large :math:`|x|`. *)

val sindg : float -> float
(** Sine of angle given in degrees. *)

val cosdg : float -> float
(** Cosine of the angle given in degrees. *)

val tandg : float -> float
(** Tangent of angle given in degrees. *)

val cotdg : float -> float
(** Cotangent of the angle given in degrees. *)

val hypot : float -> float -> float
(** ``hypot x y`` returns :math:`\sqrt{x^2 + y^2}`. *)

val xlogy : float -> float -> float
(** ``xlogy(x, y)`` returns :math:`x \log(y)`. *)

val xlog1py : float -> float -> float
(** ``xlog1py(x, y)`` returns :math:`x \log(y+1)`. *)

val logit : float -> float
(** ``logit(x)`` returns :math:`\log[p/(1-p)]`. *)

val expit : float -> float
(** ``expit(x)`` returns :math:`1/(1+\exp(-x))`. *)

val log1mexp : float -> float
(** ``log1mexp(x)`` returns :math:`log(1-exp(x))`. *)

val log1pexp : float -> float
```

## Special Functions

### Airy Functions

```
val airy : float -> float * float * float * float
(**
Airy function ``airy x`` returns ``(Ai, Ai', Bi, Bi')`` evaluated at :math:`x`.
``Ai'`` is the derivative of ``Ai`` whilst ``Bi'`` is the derivative of ``Bi``.
*)
```

### Bessel Functions 

```
val j0 : float -> float
(** Bessel function of the first kind of order 0. *)

val j1 : float -> float
(** Bessel function of the first kind of order 1. *)

val jv : float -> float -> float
(** Bessel function of real order. *)

val y0 : float -> float
(** Bessel function of the second kind of order 0. *)

val y1 : float -> float
(** Bessel function of the second kind of order 1. *)

val yv : float -> float -> float
(** Bessel function of the second kind of real order. *)

val yn : int -> float -> float
(** Bessel function of the second kind of integer order. *)

val i0 : float -> float
(** Modified Bessel function of order 0. *)

val i0e : float -> float
(** Exponentially scaled modified Bessel function of order 0. *)

val i1 : float -> float
(** Modified Bessel function of order 1. *)

val i1e : float -> float
(** Exponentially scaled modified Bessel function of order 1. *)

val iv : float -> float -> float
(** Modified Bessel function of the first kind of real order. *)

val k0 : float -> float
(** Modified Bessel function of the second kind of order 0, :math:`K_0`.*)

val k0e : float -> float
(** Exponentially scaled modified Bessel function K of order 0. *)

val k1 : float -> float
(** Modified Bessel function of the second kind of order 1, :math:`K_1(x)`. *)

val k1e : float -> float
(** Exponentially scaled modified Bessel function K of order 1. *)
```

### Elliptic Functions 

```
val ellipj : float -> float -> float * float * float * float
(** Jacobian Elliptic function ``ellipj u m`` returns ``(sn, cn, dn, phi)``. *)

val ellipk : float -> float
(** ``ellipk m`` returns the complete elliptic integral of the first kind. *)

val ellipkm1 : float -> float
(** FIXME. Complete elliptic integral of the first kind around :math:`m = 1`. *)

val ellipkinc : float -> float -> float
(** ``ellipkinc phi m`` incomplete elliptic integral of the first kind. *)

val ellipe : float -> float
(** ``ellipe m`` complete elliptic integral of the second kind. *)

val ellipeinc : float -> float -> float
(** ``ellipeinc phi m`` incomplete elliptic integral of the second kind. *)
```

### Gamma Functions

```
val gamma : float -> float
(**
``gamma z`` returns the value of the Gamma function.
The gamma function is often referred to as the generalized factorial since
:math:`z\ gamma(z) = \gamma(z+1)` and :math:`gamma(n+1) = n!`
for natural number :math:`n`.
 *)

val rgamma : float -> float
(** Reciprocal Gamma function. *)

val loggamma : float -> float
(** Logarithm of the gamma function. *)

val gammainc : float -> float -> float
(** Incomplete gamma function. *)

val gammaincinv : float -> float -> float
(** Inverse function of ``gammainc``. *)

val gammaincc : float -> float -> float
(** Complemented incomplete gamma integral. *)

val gammainccinv : float -> float -> float
(** Inverse function of ``gammaincc``. *)

val psi : float -> float
(** The digamma function. *)
```

### Beta Functions

```
val beta : float -> float -> float
(**
Beta function.
 *)

val betainc : float -> float -> float -> float
(** Incomplete beta integral. *)

val betaincinv : float -> float -> float -> float
(** Inverse function of ``betainc``. *)
```

### Factorials

```
val fact : int -> float
(** Factorial function ``fact n`` calculates :math:`n!`. *)

val log_fact : int -> float
(** Logarithm of factorial function ``log_fact n`` calculates :math:`\log n!`. *)

val doublefact : int -> float
(** Double factorial function ``doublefact n`` calculates
:math:`n!! = n(n-2)(n-4)\dots 2` or :math:`\dots 1` *)

val log_doublefact : int -> float
(** Logarithm of double factorial function. *)

val permutation : int -> int -> int
(** ``permutation n k`` returns the number :math:`n!/(n-k)!` of ordered subsets
 * of length :math:`k`, taken from a set of :math:`n` elements. *)

val permutation_float : int -> int -> float
(**
``permutation_float`` is like ``permutation`` but deals with larger range.
 *)

val combination : int -> int -> int
(** ``combination n k`` returns the number :math:`n!/(k!(n-k)!)` of subsets of k elements
    of a set of n elements. This is the binomial coefficient
    :math:`\binom{n}{k}` *)

val combination_float : int -> int -> float
(** ``combination_float`` is like ``combination`` but can deal with a larger range. *)

val log_combination : int -> int -> float
(** ``log_combination n k`` returns the logarithm of :math:`\binom{n}{k}`. *)
```

### Error Functions 

```
val erf : float -> float
(** Error function. :math:`\int_{-\infty}^x \frac{1}{\sqrt(2\pi)} \exp(-(1/2) y^2) dy` *)

val erfc : float -> float
(** Complementary error function, :math:`\int^{\infty}_x \frac{1}{\sqrt(2\pi)} \exp(-(1/2) y^2) dy` *)

val erfcx : float -> float
(** Scaled complementary error function, :math:`\exp(x^2) \mathrm{erfc}(x)`. *)

val erfinv : float -> float
(** Inverse function of ``erf``. *)

val erfcinv : float -> float
(** Inverse function of ``erfc``. *)
```

### Struve Functions

```
val struve : float -> float -> float
(** ``struve v x`` returns the value of the Struve function of
order :math:`v` at :math:`x`. The Struve function is defined as,

.. math::
  H_v(x) = (z/2)^{v + 1} \sum_{n=0}^\infty \frac{(-1)^n (z/2)^{2n}}{\Gamma(n + \frac{3}{2}) \Gamma(n + v + \frac{3}{2})},

where :math:`\Gamma` is the gamma function. :math:`x` must be positive unless :math:`v` is an integer

 *)
```

### Zeta Functions 

```
val zeta : float -> float -> float
(** ``zeta x q`` returns the Hurwitz zeta function :math:`\zeta(x, q)`, which
    reduces to the Riemann zeta function :math:`\zeta(x)` when :math:`q=1`. *)

val zetac : float -> float
(** Riemann zeta function minus 1. *)
```

### Other Functions

```
val is_prime : int -> bool
(** returns true if x is a prime number. 
The function is deterministic for all numbers representable by an int. The function uses the Rabin-Miller primality test.
*)

val fermat_fact : int -> int * int
(**
``fermat_fact x`` performs Fermat factorisation over ``x``, i.e. into two
roughly equal factors. ``x`` must be an odd number.
 *)

val nextafter : float -> float -> float
(** ``nextafter from to`` returns the next representable double precision value
of ``from`` in the direction of ``to``. If ``from`` equals ``to``, this value
is returned.
 *)

val nextafterf : float -> float -> float
(** ``nextafter from to`` returns the next representable single precision value
of ``from`` in the direction of ``to``. If ``from`` equals ``to``, this value
is returned.
 *)

```

## Interpolation and Extrapolation

## Integration

### Dawson and Fresnel Integrals

```
val dawsn : float -> float
(** Dawson's integral. *)

val fresnel : float -> float * float
(** Fresnel trigonometric integrals. ``fresnel x`` returns a tuple consisting of
``(Fresnel sin integral, Fresnel cos integral)``. *)
```

### Other Special Integrals

```
val expn : int -> float -> float
(** Exponential integral :math:`E_n`. *)

val shichi : float -> float * float
(** Hyperbolic sine and cosine integrals, ``shichi x`` returns
 * :math:`(\mathrm{shi}, \mathrm{chi})``. *)

val shi : float -> float
(** Hyperbolic sine integral. *)

val chi : float -> float
(** Hyperbolic cosine integral. *)

val sici : float -> float * float
(** Sine and cosine integrals, ``sici x`` returns :math:`(\mathrm{si}, \mathrm{ci})`. *)

val si : float -> float
(** Sine integral. *)

val ci : float -> float
(** Cosine integral. *)
```

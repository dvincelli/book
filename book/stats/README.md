# Statistical Functions

Statistics is an indispensable tool for data analysis, it helps us to gain the insights from data. The statistical functions in Owl can be categorised into three groups: descriptive statistics, distributions, and hypothesis tests.

## Random Variables

We start from assigning probabilities to *events*.
A event may comprise of finite or infinite number of possible outcomes. All possible output make up the *sample space*.
To better capture this assigning processes, we need the idea of *Random Variables*.

A random variable is a function that associate sample output of events with some numbers of interests.
Imagine the classic tossing coin game, we toss the coin four times, and the result is "head", "head", "tail", "head".
We are interested in the number of "head" in this outcome. So we make a Random Variable "X" to denote this number, and `X(["head", "head", "tail", "head"]) = 3`.
You can see that using random variables can greatly reduce the event sample space.

Depending on the number of values it can be, a random variable can be broadly categorised into *Discrete* Random Variable (with finite number of possible output), and *Continuous* Random Variable (with infinite number of possible output).

### Discrete Random Variables

Back to the coin tossing example. Suppose that the coin is specially minted so that the probability of tossing head is $p$.
In this scenario, we toss for three times.
Use the number of heads as a random variable $X$, and it contains four possible outcomes: 0, 1, 2, or 3.

We can calculate the possibility of each output result. Since each toss is a individual trial, the possibility of three heads `P(X=2)` is $p^3$.
Two heads includes three cases: HHT, HTH, THH, each has a probability of $p^2(1-p)$, and together $P(X=2) = 3p^2(1-p)$.
Similarly $P(X=1)=3p(1-p)^2$, and $P(X=0)=(1-p)^3$.

Formally, consider a series of $n$ independent trails, each trail containing two possible results, and the result of interest happens at a possibility of $p$, then the possibility distribution of random variable $X$ is ($X$ being the number of result of interests):

$$P(X=k) = {N\choose k} p^k(1-p)^{n-k}.$$ {#eq:stats:binomial_pdf}

This type of distribution is called the *Binomial Probability Distribution*.
We can stimulate this process of tossing coins with the `Stats.binomial_rvs` function.
Suppose the probability of tossing head is 0.4, and for 10 times.

```ocaml
# let _ =
    let toss = Array.make 10 0 in
    Array.map (fun _ -> Stats.binomial_rvs 0.3 1) toss
- : int array = [|0; 0; 0; 0; 0; 0; 0; 0; 1; 0|]
```

The equation [@eq:stats:binomial_pdf] is called the *&probability density function* (PDF) of this binomial distribution.
Formally the PDF of random variable X is denoted with $p_X(k)$ and is defined as:

$$p_X(k)=P({s \in S | X(s) = k}),$$

where $S$ is the sample space.
This can also be expressed with the code:

```ocaml
# let x = [|0; 1; 2; 3|]
val x : int array = [|0; 1; 2; 3|]
# let p = Array.map (Stats.binomial_pdf ~p:0.3 ~n:3) x
val p : float array =
  [|0.342999999999999916; 0.440999999999999837; 0.188999999999999918;
    0.0269999999999999823|]
# Array.fold_left (+.) 0. p
- : float = 0.999999999999999778
```

Aside from the PDF, another related and frequently used idea is to see the probability of random variable $X$ being within a certain range: $P(a \leq X \leq b)$.
It can be rewritten as $P(X \leq b) - P(X \leq a - 1)$.
Here the term $P(X \leq t)$ is called the *Cumulative Distribution Function* of random variable $X$.
For the binomial distribution, it CDF is:

$$p(X\leq~k)=\sum_{i=0}^k{N\choose i} p^k(1-p)^{n-i}.$$

We can calculate the CDF in the 3-tossing problem with code again.

```ocaml
# let x = [|0; 1; 2; 3|]
val x : int array = [|0; 1; 2; 3|]
# let p = Array.map (Stats.binomial_cdf ~p:0.3 ~n:3) x
val p : float array = [|0.342999999999999972; 0.784; 0.973; 1.|]
```

### Continuous Random Variables

Unlike discrete random variable, a continuous random variable has infinite number of possible outcomes.
For example, in uniform distribution, we can pick a random real number between 0 and 1. Apparently there can be infinite number of outputs.

One of the most widely used continuous distribution is no doubt the *Gaussian distribution*.
It's probability function is a continuous one:
$$p(x) = \frac{1}{\sqrt{2\pi~\delta}}s\exp{-\frac{1}{2}\left(\frac{t - \mu}{\sigma}\right)^2}$$ {#eq:stats:gaussian_pdf}

Here the $\mu$ and $\sigma$ are parameters. Depending on them, the $p(x)$ can take different shapes.
Let's look at an example.

We generate two data sets in this example, and both contain 999 points drawn from different Gaussian distribution $\mathcal{N} (\mu, \sigma^{2})$. For the first one, the configuration is $(\mu = 1, \sigma = 1)$; whilst for the second one, the configuration is $(\mu = 12, \sigma = 3)$.

```ocaml env=stats_02
let noise sigma = Stats.gaussian_rvs ~mu:0. ~sigma;;
let x = Array.init 999 (fun _ -> Stats.gaussian_rvs ~mu:1. ~sigma:1.);;
let y = Array.init 999 (fun _ -> Stats.gaussian_rvs ~mu:12. ~sigma:3.);;
```

We can visualise the data sets using histogram plot as below. When calling `histogram`, we also specify 30 bins explicitly. You can also fine tune the figure using `spec` named parameter to specify the colour, x range, y range, and etc. We will discuss in details on how to use Owl to plot in a separate chapter.

```ocaml env=stats_02
(* convert arrays to matrices *)

let x' = Mat.of_array x 1 999;;
let y' = Mat.of_array y 1 999;;

(* plot the figures *)

let h = Plot.create ~m:1 ~n:2 "plot_02.png" in

Plot.subplot h 0 0;
Plot.set_ylabel h "frequency";
Plot.histogram ~bin:30 ~h x';
Plot.histogram ~bin:30 ~h y';

Plot.subplot h 0 1;
Plot.set_ylabel h "PDF p(x)";
Plot.plot_fun ~h (fun x -> Stats.gaussian_pdf ~mu:1. ~sigma:1. x) (-2.) 6.;
Plot.plot_fun ~h (fun x -> Stats.gaussian_pdf ~mu:12. ~sigma:3. x) 0. 25.;

Plot.output h;;
```

In subplot 1, we can see the second data set has much wider spread. In subplot 2, we also plot corresponding the probability density functions of the two data sets.

![Probability density functions of two data sets](images/stats/plot_02.png "plot 02"){ width=90% #fig:stats:plot_02 }

The CDF of Gaussian can be calculated with infinite summation, i.e. integration:

$$p(x\leq~k)=\frac{1}{\sqrt{2\pi}}\int_{-\infty}^k~e^{-t^2/2}dt.$$

We can observe this function with `gaussian_cdf`.

```ocaml env=stats_02
let h = Plot.create "plot_gaussian_cdf.png" in
Plot.set_ylabel h "CDF";
Plot.plot_fun ~h ~spec:[ RGB (66,133,244); LineStyle 1; LineWidth 2.; Marker "*" ] (fun x -> Stats.gaussian_cdf ~mu:1. ~sigma:1. x) (-2.) 6.;
Plot.plot_fun ~h ~spec:[ RGB (219,68,55);  LineStyle 2; LineWidth 2.; Marker "+" ] (fun x -> Stats.gaussian_cdf ~mu:12. ~sigma:3. x) 0. 25.;
Plot.(legend_on h ~position:SouthEast [|"mu=1,sigma=1"; "mu=12, sigma=3"|]);
Plot.output h
```

![Cumulated density functions of two data sets](images/stats/plot_gaussian_cdf.png "plot gaussian cdf"){ width=70% #fig:stats:plot_gaussian_cdf }

### Descriptive Statistics

Mean, Variance: Definition, and math derivation of Gaussian as examples.

The definition of moments, and higher moments.

Descriptive statistics are used to summarise the characteristics of data. The commonly used ones are mean, variance, standard deviation, skewness, kurtosis, and etc.

We first draw one hundred random numbers which are uniformly distributed between 0 and 10. Here we use `Stats.uniform_rvs` function to generate numbers following uniform distribution.

```ocaml env=stats_00
let data = Array.init 100 (fun _ -> Stats.uniform_rvs 0. 10.);;
```

Then We use `mean` function calculate sample average. As we can see, it is around 5. We can also calculate higher moments such as variance and skewness easily with corresponding functions.

```ocaml env=stats_00
# Stats.mean data
- : float = 5.18160409659184573
# Stats.std data
- : float = 2.92844832850280135
# Stats.var data
- : float = 8.57580961271085229
# Stats.skew data
- : float = -0.109699186612116223
# Stats.kurtosis data
- : float = 1.75165078829330856
```

The following code calculates different central moments of `data`. A central moment is a moment of a probability distribution of a random variable about the random variable's mean. The zero-th central moment is always 1, and the first is close to zero, and the second is close to the variance.

```ocaml env=stats_00
# Stats.central_moment 0 data
- : float = 1.
# Stats.central_moment 1 data
- : float = -3.13082892944294137e-15
# Stats.central_moment 2 data
- : float = 8.49005151658374224
# Stats.central_moment 3 data
- : float = -2.75496511397836663
```

### Order Statistics

Order statistics and rank statistics are among the most fundamental tools in non-parametric statistics and inference. The $k^{th}$ order statistic of a statistical sample is equal to its kth-smallest value. The example functions of ordered statistics are as follows.

```ocaml
Stats.min;; (* the mininum of the samples *)
Stats.max;; (* the maximum of the samples *)
Stats.median;; (* the median value of the samples *)
Stats.quartile;; (* quartile of the samples *)
Stats.first_quartile;; (* the first quartile of the samples *)
Stats.third_quartile;; (* the third quartile of the samples *)
Stats.interquartile;; (* the interquartile of the samples *)
Stats.percentile;; (* percentile of the samples *)
```

In addition to the aforementioned ones, there are many other ordered statistical functions in Owl for you to explore.

IMAGE to show the difference of mean, median, mode, etc.

## Special Distribution

illustrate the naming convention.

* `gaussian_rvs` : random number generator.
* `gaussian_pdf` : probability density function.
* `gaussian_cdf` : cumulative distribution function.
* `gaussian_ppf` : percent point function (inverse of CDF).
* `gaussian_sf` : survival function (1 - CDF).
* `gaussian_isf` : inverse survival function (inverse of SF).
* `gaussian_logpdf` : logarithmic probability density function.
* `gaussian_logcdf` : logarithmic cumulative distribution function.
* `gaussian_logsf` : logarithmic survival function.

Stats module supports many distributions. For each distribution, there is a set of related functions using the distribution name as their common prefix.

TODO: Add Poisson Distribution Implementation

### Gamma Distribution

Definition

PDF, CDF

Application

### Beta Distribution

### Chi-Square Distribution

### Student-t Distribution

### Cauchy Distribution

## Multiple Variables

Joint Density

Independence of random variables

Mean and Variance

Multinomial distribution

## Hypothesis Tests

### Theory

While descriptive statistics solely concern properties of the observed data, statistical inference focusses on studying whether the data set is sampled from a larger population. In other words, statistical inference make propositions about a population. Hypothesis test is an important method in inferential statistical analysis. There are two hypotheses proposed with regard to the statistical relationship between data sets.

* Null hypothesis $H_0$: there is no relationship between two data sets.
* Alternative hypothesis $H_1$: there is statistically significant relationship between two data sets.

Type I and Type II errors.

### Gaussian Distribution in Hypothesis Testing

The `Stats` module in Owl supports many different kinds of hypothesis tests.

* Z-Test
* Student's T-Test
* Paired Sample T-Test
* Unpaired Sample T-Test
* Kolmogorov-Smirnov Test
* Chi-Square Variance Test
* Jarque-Bera Test
* Fisher's Exact Test
* Wald–Wolfowitz Runs Test
* Mann-Whitney Rank Test
* Wilcoxon Signed-rank Test

Now let's see how to perform a z-test in Owl. We first generate two data sets, both are drawn from Gaussian distribution but with different parameterisation. The first one `data_0` is drawn from $\mathcal{N}(0, 1)$, while the second one `data_1` is drawn from $\mathcal{N}(3, 1)$.

```ocaml env=stats_03
let data_0 = Array.init 10 (fun _ -> Stats.gaussian_rvs ~mu:0. ~sigma:1.);;
let data_1 = Array.init 10 (fun _ -> Stats.gaussian_rvs ~mu:3. ~sigma:1.);;
```

Our hypothesis is that the data set is drawn from Gaussian distribution $\mathcal{N}(0, 1)$. From the way we generated the synthetic data, it is obvious that `data_0` will pass the test, but let's see what Owl will test us using its `Stats.z_test` function.

```ocaml env=stats_03
# Stats.z_test ~mu:0. ~sigma:1. data_0
- : Owl_stats.hypothesis =
{Owl.Stats.reject = false; p_value = 0.289340080583773251;
 score = -1.05957041132113083}
```

The returned result is a record with the following type definition. The fields are self-explained: `reject` field tells whether the null hypothesis is rejected, along with the p value and score calculated with the given data set.

```ocaml
type hypothesis = {
  reject : bool;
  p_value : float;
  score : float;
}
```

From the previous result, we can see `reject = false`, indicating null hypothesis is rejected, therefore the data set `data_0` is drawn from $\mathcal{N}(0, 1)$. How about the second data set then?

```ocaml env=stats_03
# Stats.z_test ~mu:0. ~sigma:1. data_1
- : Owl_stats.hypothesis =
{Owl.Stats.reject = true; p_value = 5.06534675819424548e-23;
 score = 9.88035435799393547}
```

As we expected, the null hypothesis is accepted with a very small p value. This indicates that `data_1` is drawn from a different distribution rather than assumed $\mathcal{N}(0, 1)$.

### Two-Sample Inferences

### Goodness-of-fit Tests

### Non-parametric Statistics

Wilcoxon Tests


## Covariance and Correlations

Correlation studies how strongly two variables are related. There are different ways of calculating correlation. For the first example, let's look at Pearson correlation.

`x` is our explanatory variable and we draw 50 random values uniformly from an interval between 0 and 10. Both `y` and `z` are response variables with a linear relation to `x`. The only difference is that we add different level of noise to the response variables. The noise values are generated from Gaussian distribution.

```ocaml env=stats_01
let noise sigma = Stats.gaussian_rvs ~mu:0. ~sigma;;
let x = Array.init 50 (fun _ -> Stats.uniform_rvs 0. 10.);;
let y = Array.map (fun a -> 2.5 *. a +. noise 1.) x;;
let z = Array.map (fun a -> 2.5 *. a +. noise 8.) x;;
```

It is easier to see the relation between two variables from a figure. Herein we use Owl's Plplot module to make two scatter plots.

```ocaml env=stats_01
(* convert arrays to matrices *)

let x' = Mat.of_array x 1 50;;
let y' = Mat.of_array y 1 50;;
let z' = Mat.of_array z 1 50;;

(* plot the figures *)

let h = Plot.create ~m:1 ~n:2 "plot_01.png" in

  Plot.subplot h 0 0;
  Plot.set_xlabel h "x";
  Plot.set_ylabel h "y (sigma = 1)";
  Plot.scatter ~h x' y';

  Plot.subplot h 0 1;
  Plot.set_xlabel h "x";
  Plot.set_ylabel h "z (sigma = 8)";
  Plot.scatter ~h x' z';

  Plot.output h;;
```

The subfigure 1 shows the functional relation between `x` and `y` whilst the subfiture 2 shows the relation between `x` and `z`. Because we have added higher-level noise to `z`, the points in the second figure are more diffused.

![Functional relation between `x` and the other two variables.](images/stats/plot_01.png "plot 01"){ width=90% #fig:stats:plot_01 }


Intuitively, we can easily see there is stronger relation between `x` and `y` from the figures. But how about numerically? In many cases, numbers are preferred because they are easier to compare with by a computer. The following snippet calculates the Pearson correlation between `x` and `y`, as well as the correlation between `x` and `z`. As we see, the smaller correlation value indicates weaker linear relation between `x` and `z` comparing to that between `x` and `y`.

```ocaml env=stats_01
# Stats.corrcoef x y
- : float = 0.991145445979576656
# Stats.corrcoef x z
- : float = 0.692163016204755288
```

## Analysis of Variance

## Summary

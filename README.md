\# Gaussian / Non-Gaussian Photonic-State Simulator



An interactive Jupyter/QuTiP teaching simulator for exploring Gaussian and non-Gaussian quantum states of light using three complementary views:



\- Wigner phase-space distributions

\- Fock-state probability distributions

\- Gaussian moment-residual diagnostics



The simulator is intended for photonics and quantum-optics instruction. It exposes both physical state parameters and numerical controls so that students can distinguish physical behavior from numerical artifacts.



\---



\## Overview



The notebook includes the following photonic states:



\- Vacuum

\- Coherent state

\- Squeezed vacuum

\- Fock state

\- Vacuum/single-photon mixture



The mixed state is



```text

rho = (1 - p1)|0><0| + p1|1><1|

```



For each selected state, the simulator displays:



1\. Wigner function W(x,p)

2\. Fock probabilities P\_n

3\. Relative Gaussian moment residuals delta\_n



The interface also reports:



\- Gaussian / non-Gaussian classification

\- Wigner positivity at a user-selected numerical cutoff

\- Minimum Wigner value

\- Purity



The default comparison view shows four states:



```text

Coherent

Squeezed vacuum

Fock

Vacuum/single-photon mixture

```



Vacuum can be selected separately or included in the five-state comparison.



\---



\## Educational Goals



The simulator is designed to make several distinctions explicit:



\- Gaussianity is a property of the state in phase space.

\- Wigner positivity does not generally imply Gaussianity for mixed states.

\- Coherent and squeezed-vacuum states are Gaussian.

\- Fock states are non-Gaussian and exhibit Wigner negativity.

\- A mixed state can be non-Gaussian while retaining an everywhere nonnegative Wigner function.

\- Finite Hilbert-space truncation can create artificial high-order moment violations and apparent Wigner negativity.

\- Numerical convergence and physical interpretation must be assessed separately.



The notebook therefore functions both as a quantum-optics simulator and as an exercise in numerical judgment.



\---



\## Requirements



The notebook assumes a Python/Jupyter environment with:



```text

numpy

matplotlib

qutip

ipywidgets

IPython

```



A typical installation is:



```bash

pip install numpy matplotlib qutip ipywidgets jupyter

```



Then start Jupyter with either:



```bash

jupyter lab

```



or:



```bash

jupyter notebook

```



\---



\## Simulator Structure



The notebook is organized as a validated computational stack:



```text

Cell 1  State construction

Cell 2  Quadrature moments and Gaussianity

Cell 3  Wigner diagnostics and negativity cutoff

Cell 4  Plotting and visualization

Cell 5  Interactive comparison UI

```



The current version contains 54 unit and regression tests.



\---



\## Physical Conventions



The quadratures are defined as:



```text

X = (a + a^dagger) / sqrt(2)



P = (a - a^dagger) / (i sqrt(2))

```



Therefore:



```text

\[X,P] = i

```



and the vacuum variances are:



```text

Var(X) = Var(P) = 1/2

```



The rotated quadrature is:



```text

X\_theta = X cos(theta) + P sin(theta)

```



\---



\## Gaussian Moment Diagnostic



For each quadrature direction, the simulator evaluates central moments:



```text

mu\_n(theta) =

&#x20;   < (X\_theta - <X\_theta>)^n >

```



For a Gaussian distribution:



```text

mu\_(2k+1) = 0

```



and:



```text

mu\_(2k) = (2k - 1)!! \* mu\_2^k

```



The simulator plots normalized residuals that vanish for an ideal Gaussian state.



For odd orders:



```text

delta\_n = mu\_n / mu\_2^(n/2)

```



For even orders:



```text

delta\_n =

&#x20;   mu\_n / \[ (n - 1)!! \* mu\_2^(n/2) ] - 1

```



The Gaussianity classifier uses the largest absolute residual over the selected moment orders and quadrature directions.



Because only a finite set of moments is evaluated, this is a numerical Gaussianity diagnostic rather than a formal mathematical proof.



\---



\## Wigner Function



Using the quadrature convention above, the vacuum Wigner function is:



```text

W\_0(x,p) =

&#x20;   exp\[-(x^2 + p^2)] / pi

```



For the single-photon Fock state:



```text

W\_1(x,p) =

&#x20;   \[2(x^2 + p^2) - 1]

&#x20;   exp\[-(x^2 + p^2)] / pi

```



At the origin:



```text

W\_1(0,0) = -1/pi

```



This provides a simple analytic benchmark for the numerical Wigner calculation.



\---



\## Positive-Wigner Non-Gaussian Mixed State



A particularly useful teaching example is:



```text

rho =

&#x20;   (1 - p1)|0><0|

&#x20;   + p1|1><1|

```



Its Wigner function is:



```text

W(x,p) =

&#x20;   exp(-r^2) / pi

&#x20;   \* \[1 - 2 p1 + 2 p1 r^2]



where



r^2 = x^2 + p^2

```



At the origin:



```text

W(0,0) = (1 - 2 p1) / pi

```



Therefore:



```text

p1 < 1/2    Wigner function is nonnegative

p1 = 1/2    Wigner function touches zero at the origin

p1 > 1/2    Wigner negativity appears

```



The state remains non-Gaussian for any p1 > 0.



This gives a direct counterexample to the incorrect inference:



```text

W(x,p) >= 0  =>  Gaussian state

```



For pure states, the connection is stronger: Hudson's theorem states that a pure state with an everywhere nonnegative Wigner function is Gaussian.



\---



\## Squeezed Vacuum and dB Input



Squeezing is entered in decibels.



The simulator uses:



```text

S\_dB = 10 log10(V\_vac / V\_sq)

```



with:



```text

V\_sq / V\_vac = exp(-2r)

```



Therefore:



```text

S\_dB = 20 r / ln(10)

```



and:



```text

r = \[ln(10) / 20] S\_dB

```



For example:



```text

6 dB  ->  r = 0.690776

```



The derived value of r is displayed in the UI.



\---



\## Numerical Controls



\### Hilbert-space dimension



The Fock-space dimension N is user controlled.



Increasing coherent amplitude or squeezing generally requires a larger basis. High-order moments are especially sensitive to truncated high-n tails.



Students should increase N until the Wigner minimum and moment residuals stop changing appreciably.



This makes basis convergence an explicit part of the exercise rather than a hidden implementation detail.



\### Wigner negativity cutoff



The default numerical cutoff is:



```text

1e-7

```



The raw minimum Wigner value is always reported.



Resolved Wigner negativity is declared only when:



```text

W\_min < -cutoff

```



The cutoff is user adjustable.



This allows students to distinguish between a mathematically negative floating-point result and a physically resolved negative Wigner value.



\### Quadrature directions



The number of sampled quadrature directions can be varied.



The directions are sampled uniformly over:



```text

0 <= theta < pi

```



\### Moment y-axis range



The default moment-residual scale is at least:



```text

\[-1, +1]

```



This prevents tiny numerical residuals in Gaussian states from appearing visually significant.



The y-axis range can also be set manually for closer inspection.



\### Phase-space extent and grid



The phase-space plotting domain and Wigner-grid resolution are user adjustable.



\---



\## Suggested Student Investigations



1\. Compare vacuum, coherent, and squeezed-vacuum states. Identify their common Gaussian signatures.



2\. Change the coherent-state amplitude and phase. Track the displacement of the Wigner function and changes in the Fock distribution.



3\. Change squeezing in dB. Observe the phase-space deformation and the increasing support over even Fock states.



4\. Compare several Fock states. Examine their Wigner functions and Gaussian moment residuals.



5\. Verify that parity-symmetric Fock states have vanishing odd central-moment residuals while remaining strongly non-Gaussian.



6\. Vary p1 in the vacuum/single-photon mixture and locate the Wigner-negativity threshold at p1 = 1/2.



7\. Find a mixed state that is non-Gaussian but has an everywhere nonnegative Wigner function.



8\. Increase squeezing while keeping the Hilbert-space dimension fixed. Identify numerical truncation artifacts.



9\. Increase the Hilbert-space dimension until the squeezed-state Wigner minimum and moment residuals converge.



10\. Vary the Wigner-negativity cutoff. Discuss when a small negative numerical value should be regarded as resolved negativity.



Detailed expected outcomes and explanations are provided in the accompanying teaching note.



\---



\## Recommended Teaching Sequence



A useful progression is:



```text

Vacuum

&#x20; |

&#x20; v

Coherent state

&#x20; |

&#x20; v

Squeezed vacuum

&#x20; |

&#x20; v

Fock state

&#x20; |

&#x20; v

Mixed state

```



The first three establish the Gaussian family.



The Fock state introduces clear non-Gaussianity and Wigner negativity.



The mixed state then demonstrates that Wigner positivity and Gaussianity are not equivalent for mixed states.



\---



\## Validation



The simulator was developed using analytic and numerical regression tests.



Validated checks include:



\- Fock-state normalization

\- Coherent-state Fock recurrence

\- Squeezed-vacuum even-parity structure

\- Coherent and squeezed analytic quadrature variances

\- Gaussian moment recursion

\- Analytic vacuum and single-photon Wigner functions

\- Wigner normalization

\- Single-photon negative volume

\- Exact p1 = 1/2 mixed-state negativity threshold

\- Wigner-cutoff propagation

\- User-controlled Hilbert-space dimension

\- Comparison plotting

\- dB-to-r conversion

\- UI state-selection logic



Current validation status:



```text

54 / 54 tests passing

```



\---



\## Interpretation



The simulator deliberately separates three questions:



```text

1\. Is the state Gaussian?



2\. Is its Wigner function nonnegative at the selected

&#x20;  numerical resolution?



3\. Is the numerical representation converged?

```



These questions should not be collapsed into a single classification.



For example, apparent Wigner negativity in a strongly squeezed state can result from insufficient Hilbert-space dimension. Increasing the basis size is then a convergence test, not a change in physical state.



\---



\## Repository Files



A typical repository can contain:



```text

README.md

Gaussian\_nonGaussian\_photonic\_states.ipynb

teaching\_note.pdf

```



Rename the notebook and teaching-note entries above to match the final repository filenames.



\---



\## References



The accompanying teaching note contains the full reference list and derivations.



It includes background references on quantum optics, coherent and squeezed states, Wigner functions, Gaussian quantum information, and Hudson's theorem.



\---



\## Author



\*\*Boris Kiefer\*\*  

New Mexico State University



GitHub: https://github.com/boriskiefer



\---



\## Use



This material is intended for teaching, research training, and exploration in quantum optics and photonics.



If you use or adapt the simulator in a course, student project, or research-training activity, please cite the repository and accompanying teaching note.




# BH_DVCS_Proton
Coding up the huge contributions (BH, DVCS, interference) to the cross section during DIS Proton scattering.

## (1): Procedure

Modern physics is hard. To understand the internal structure of the proton, we rely on what's called "Hadronic Theory," which employs the techniques of Quantum Field Theory but remains noncommittal about what the proton/neutron is composed of. Following through with this enables calculation of a quantity -- called a "cross section" -- that has a physical significance.

### (2): Calculating the Derived Kinematics

The first thing we need to calculate is something called the "derived kinematics." These are just quantities that better parameterize/quantify the scattering process. It is also especially nice to calculate them because they are Lorentz invariant (frame invariant). Some of these derived kinematic quantities are functions of other derived kinematic quantities -- This will be a theme throughout this project. We cannot belligerently calculate everything all at once; There is an implied chronology in our calculations. So, the way we will list the derived kinematics is in precisely the ordered in which we decided to calculate them based on these "chronological considerations."

#### (2.1): Kinematic Invariant Epsilon: $\varepsilon (x_{B}, Q)$:

$$\varepsilon (x_{B}, Q^{2}) = \frac{2 x_{B} m_{p}}{Q}$$

#### (2.2): Kinematic Invariant Epsilon Squared: $\varepsilon^{2}(x_{B}, Q^{2})$:

This is just the squared version of the kinematic quantity above. It's nicer because it involves $Q^{2}$ rather than $Q$, which is the more natural quantity here.

$$\varepsilon^{2} (x_{B}, Q^{2}) = \frac{4 x_{B}^{2} m_{p}^{2}}{Q^{2}}.$$

#### (2.3): Lepton Energy Fraction: $y (Q^{2}, \varepsilon)$:

$$y = \frac{p_{1} \cdot q_{1}}{p_{1} \cdot k} = \frac{Q}{\varepsilon}.$$

#### (2.4): Skewness Parameter: $\xi (t, Q^{2}, x_{B})$

$$\xi = x_{B} \left( \frac{1 + \frac{t}{2 Q^{2}}}{2 - x_{B} + \frac{x_{B} t }{Q^{2}}} \right)$$.

#### (2.5): Minumum Hadron Momentum Transfer: $t_{0}(\xi)$

$$t_{0} = - \frac{4 \xi^{2} m_{p}^{2}}{1 - \xi^{2}}$$.

#### (2.6): Amplitude Squared Prefactor: $\Gamma(x_{B}, y, \varepsilon)$: 

I'm not really sure why we waited to calculate this -- it doesn't rely on derived kinematic quantities. Anyway, here it is. One more note: we are setting the electron charge to 1 for the time being.

$$\Gamma = \frac{\alpha^{3} x_{B} y}{16 \pi^{2} e^{6} \sqrt{1 + \varepsilon^{2}}}$$.

### (2.7): Suppressed Kinematic $K$ in Fourier Mode Expansion: $K^{2}(t, Q^{2}, x_{B}, y, \varepsilon)$:

$$K^{2} = -\frac{t}{Q^{2}} (1 - x_{B})(1 - y - \frac{y^{2} \varepsilon^{2}}{4})(1 - \frac{t_{0}}{t})$$

### (3): Calculating Form Factors

Another thing that we will have to do is calculate the form factors that parameterize the structure of the hadron. Just so that we have it on record, here is how things are related (some of this was determined from experiment.)

#### (3.1): Electric Form Factor: $G_{E}(t)$:

$$G_{E}(t)= \frac{1}{(1 - \frac{t}{0.710649})^{2}}$$

#### (3.2): Magnetic Form Factor: $G_{M}(t)$:

The Magnetic Form Factor relies on the nuclear magnetic moment and the Electric Form Factor. Here is that "chronology" coming into play again.

$$G_{M}(t)= \mu G_{E}

For reference, we have $\mu = 2.792847337$.

#### (3.3): Pauli Form Factor: $F_{2}(G_{M}, G_{E})$

We needed to have calculated $G_{M}$ and $G_{E}$ before to get this.

$$F_{2} =\frac{G_{M} - G_{E}}{1 - \frac{t}{4 m_{p}^{2}}}$$

#### (3.4): Dirac Form Factor: $F_{1}(G_{M}, F_{2})$

We needed to have calculated $G_{M}$ and $F_{2}$ before.

$$F_{1} = G_{M} - F_{2}$$.

That concludes all the necessary calculations that will then play a role in the actual numerics of the amplitude contributions.

### (4): Bethe-Heitler Amplitude Squared, $|{\mathcal{M}_{BH}}|^{2}$

The squared-amplitude of the Bethe-Heitler process looks as follows:

$$|T_{BH}^{2}| = \frac{e^{6}}{x_{B}^{2} y^{2} (1 + \varepsilon^{2})^{2} \Delta^{2} P_{1}(\phi) P_{2}(\phi)} (c_{0}^{BH} + \sum_{n = 1}^{2} c_{n}^{BH} cos(n \phi) + s_{1}^{BH} sin(\phi))$$

Of course, there are tons of contributing things in this amplitude. We need to code them all. Our plan of attack is to make separate functions for everything that needs a calculation. The immediate problem is, as you might have noticed upon reading some of the literature, basically *everything depends on everything*. In order to attack this problem, we have to figure out exactly what calculation we start with, and which calculations the previous one has "opened up." As an explicit example, the two remaining lepton propagators, $P_{i}(\phi)$, in the denominator of the squared amplitude, depend on the calculation of the scalar product $k \cdot \Delta$, which depends on a ton of derived kinematic quantities. So, the rest of the this Markdown introduction is written in exact correspondence with the necessary chronology of these calculations.

#### (4.1): "K dot Delta": $k \cdot \Delta (K, \varepsilon, Q^{2}, x_{B}, y, t, \phi)$

What does this look like?

$$k \cdot \Delta (K, \varepsilon, Q^{2}, x_{B}, y, t, \phi)= - \frac{Q^{2}}{2 y (1 + \epsilon^{2})} (1 + 2 K cos(\phi) - \frac{\Delta^{2}}{Q^{2}}( 1 - x_{B} (2 - y) + \frac{y \epsilon^{2}}{2}) + \frac{y \epsilon^{2}}{2})$$.

#### (4.2): Lepton Propagator 1: $P_{1}(\phi)$

The first lepton propagator that shows up in the amplitude is

$$P_{1}(\phi)= 1 + 2 \frac{k\cdot \Delta}{Q^{2}}$$.

As you can see, we needed to calculate $k \cdot \Delta$ before approaching the propagator contributions.

#### (4.3): Lepton Propagator 2: $P_{2}(\phi)$

The second lepton propagator is:

$$P_{2}(\phi) = \frac{-2 k \cdot \Delta}{Q^{2}}+ \frac{t}{Q^{2}}$$.

#### (4.4): Fourier Mode Coefficients

##### (4.4.1): First Coefficient (Unpolarized): $c^{(0)}_{BH} (K, \varepsilon, Q^{2}, t, x_{B}, y, F_{1}, F_{2})$

##### (4.4.5): Second Coefficient (Unpolarized): $c^{(1)}_{BH} (K, \varepsilon, Q^{2}, t, x_{B}, y, F_{1}, F_{2})$

##### (4.4.6): Third Coefficient (Unpolarized): $c^{(2)}_{BH} (K, t, x_{B}, F_{1}, F_{2})$

#### (4.5): Entire Bethe-Heitler Squared-Amplitude:

We are now in a position to calculate the entire amplitude.

### (5): Deeply-Virtual Compton Scattering (DVCS) Amplitude Squared, $|{\mathcal{M}_{DVCS}}|^{2}$

Not quite ready for this yet.

### (6): Interference Contribution, $I = \mathcal{M}_{BH}^{*} \mathcal{M}_{DVCS} + \mathcal{M}_{DVCS}^{*} \mathcal{M}_{BH}$

We can also write $$I = \frac{\pm e^{6}}{x_{B} t y^{3} P_{1}(\phi) P_{2}(\phi) } (c_{0}^{I} + \sum_{n = 1}^{3} \left[ c_{n}^{I} cos(n \phi) + s_{n}^{I} sin(n \phi) \right])$$

#### (6.1): Interference Coefficient with Form Factors, $\mathcal{C}_{unp}^{I}(\mathcal{F})$

From Equation (69) in https://arxiv.org/pdf/hep-ph/0112108.pdf,

$$.

#### (6.2): Interference Coefficients:

##### (6.2.1): Interference Coefficient with Form Factors, $\Delta \mathcal{C}_{unp}^{I}(\mathcal{F})$

From Equation (72) in https://arxiv.org/pdf/hep-ph/0112108.pdf,

##### (6.2.2): Interference Coefficient with Form Factors, $\mathcal{C}_{T, unp}^{I}(\mathcal{F})$

From Equation (53) in https://arxiv.org/pdf/hep-ph/0112108.pdf,

##### (6.2.3): Interference Coefficient with Form Factors, $c_{0, unp}^{I}$

##### (6.2.4): Interference Coefficient with Form Factors, $c_{1, unp}^{I}$

##### (6.2.5): Interference Coefficient with Form Factors, $s_{1, unp}^{I}$

##### (6.2.6): Interference Coefficient with Form Factors, $c_{2, unp}^{I}$

##### (6.2.7): Interference Coefficient with Form Factors, $s_{2, unp}^{I}$

##### (6.2.8): Interference Coefficient with Form Factors, $c_{3, unp}^{I}$

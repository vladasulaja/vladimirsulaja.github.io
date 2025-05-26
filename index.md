---
title: "Vladimir Sulaja – Personal Webpage"
layout: none
---

<link rel="stylesheet" href="style.css">

<div class="navbar">
  <a href="#about">About</a>
  <a href="#projects">Projects</a>
  <a href="#contact">Contact</a>
</div>

<div class="container">

<img src="photo.jpg" alt="My Photo" style="border-radius: 8px; max-width: 120px; display: block; margin: 1rem auto;">

<h2 id="about">About Me</h2>

I hold a Ph.D. in Economics from the University of Zurich, where I conducted research in international finance, empirical banking, and the macroeconomic effects of automation. I am currently working as a Quantitative Analyst at BIT Capital.

This page features selected blog articles, models, and code that reflect my work and interests in global macroeconomics, finance, and econometrics.

<div style="margin-top: 1.5rem;">
  <a href="https://www.linkedin.com/in/vladimir-sulaja-43686550" target="_blank">
    <img src="https://cdn.jsdelivr.net/gh/simple-icons/simple-icons/icons/linkedin.svg"
         alt="LinkedIn"
         style="width: 24px; height: 24px;">
  </a>
</div>

<h2 id="projects">Projects</h2>

<div class="project">
  <h3>Consumption Slowdown after the Great Recession</h3>
  <p>
    Consumption growth in the United States slowed markedly following the 2007–2009 financial crisis. We argue that costly regulatory interventions targeting banks with foreclosure-related misconduct contributed to this decline by constraining credit supply. Using variation in county-level exposure to affected banks, we show that tighter regulatory controls reduced mortgage loan origination, leading to weaker house price recoveries and lower household wealth. We find that consumption growth slowed more in counties more exposed to these banks, consistent with a wealth effect transmitted through housing markets. The decline in mortgage lending reflects a reduction in the number of loans rather than in average loan size, suggesting that regulation operated primarily through extensive-margin credit supply.
  </p>
  <p><a href="projects/simonsulaja-consumptionslowdown.pdf" target="_blank">Read paper</a></p>
</div>

<div class="project">
  <h3>Emerging Market Business Cycles: The Story of
 Factor Shares</h3>
  <p>
    This paper investigates how differences in factor shares across countries contribute to the differences in business cycle properties between emerging market economies (EMEs) and developed economies (DEs). Using empirical data and standard international macroeconomic models, the paper shows that capital shares are strongly correlated with key moments such as the cyclicality of the trade balance and volatility of output. The findings suggest that differences in labor and capital income shares, observable across countries, play a central role in shaping macroeconomic dynamics.
  </p>
  <p><a href="projects/eme_bc.pdf" target="_blank">Read paper</a></p>
</div>

</div>

---
title: "Investment, Interest Rates, and the Overlooked Gains from Trade Balance Flexibility"
author: Vladimir Sulaja
layout: post
section: short-memos
---

## Abstract

This article explores how a flexible trade balance can significantly enhance macroeconomic stability by delinking domestic interest rates from output shocks. We show that the standard consumption risk-sharing literature underestimates the welfare gains from international financial integration by neglecting the investment channel. Using a simple small open economy DSGE model, we demonstrate that allowing for a non-zero trade balance enables economies to better manage interest rate shocks, thus facilitating higher and less volatile investment in response to productivity shifts.

## 1. Introduction

The literature on international consumption risk sharing has long concluded that the welfare gains from global financial integration are small — often less than 1% of lifetime consumption. Seminal contributions by Lucas (1987), Obstfeld and Rogoff (1995), and more recently Gourinchas and Jeanne (2006) all show that the main benefit of risk sharing arises from the smoothing of consumption over time and states of nature.

However, these studies typically neglect an important macroeconomic channel: **investment amplification through interest rate smoothing**. When the trade balance is restricted (e.g., balanced every period), countries must finance investment solely through domestic savings. This tight linkage between output, interest rates, and investment introduces fragility and lowers aggregate responsiveness to shocks.

By contrast, when the **trade balance can vary**, countries can borrow during booms (or save during busts) to stabilize interest rates and **optimize capital accumulation**. This mechanism generates a second-round welfare gain that traditional consumption-only models miss.

## 2. The Model

We employ a small open economy model with:

- **Cobb-Douglas production**:  
  \( y_t = A_t \cdot k_{t-1}^\alpha \cdot l_t^{1-\alpha} \)

- **Capital accumulation**:  
  \( k_t = (1 - \delta) \cdot k_{t-1} + i_t \)

- **Budget constraint**:  
  \( c_t + i_t + r_t \cdot b_{t-1} = y_t \)

- **Debt evolution**:  
  \( b_t = c_t + i_t + r_t \cdot b_{t-1} - y_t \)

- **Interest rate rule**:  
  \( r_t = z_t + \phi_b \cdot b_{t-1} + \eta \cdot y_t \)

Where:
- \( \phi_b \): spread sensitivity to debt  
- \( \eta \): sensitivity of interest rates to output growth

## 3. Dynare Implementation

```mod
var c y i k b r a z l;
varexo e_a e_z;

parameters beta alpha delta theta phi rho_a rho_z sigma_a sigma_z phi_b phi_L psi eta;

beta     = 0.96;
alpha    = 0.36;
delta    = 0.10;
theta    = 0.20;
phi      = 0.1;
phi_b    = 0.10;
phi_L    = 1.5;
psi      = 1.5;
rho_a    = 0.95;
rho_z    = 0.80;
sigma_a  = 0.015;
sigma_z  = 0.001;
eta      = 0.01;

model;
a = rho_a * a(-1) + e_a;
z = rho_z * z(-1) + e_z;
r = z + phi_b * b(-1) + eta * y;
y = exp(a) * k(-1)^alpha * l^(1 - alpha);
i = phi * k(-1) * ((alpha * exp(a) * k(-1)^(alpha - 1) * l^(1 - alpha)) - r - theta * l) + delta * k(-1);
k = (1 - delta) * k(-1) + i;
phi_L * l^psi = (1 - alpha) * exp(a) * k(-1)^alpha * l^(-alpha);
c + i + r * b(-1) = y;
b = c + i + r * b(-1) - y;
end;

initval;
a = 0; z = 0; k = 1; b = 0; l = 1; r = 0.04; y = 1; i = delta; c = y - i;
end;

shocks;
var e_a; stderr sigma_a;
var e_z; stderr sigma_z;
end;

steady;
check;
stoch_simul(order=1, periods=5000, drop=1000, hp_filter=1600);

## 5. Permanent Income and Risk Sharing

Standard models of international macroeconomics often evaluate the benefits of financial integration by computing the gains from consumption risk sharing. This is usually done by comparing expected utility under autarky (no financial integration) to that under perfect consumption insurance. The gains are often found to be small:

\[
\Delta U = E\left[\sum \beta^t \log(c_t^{\text{autarky}})\right] - E\left[\sum \beta^t \log(c_t^{\text{open}})\right]
\]

However, these models often abstract from the investment channel. They assume consumption is smoothed given an exogenous income process, but do not consider how smoother **interest rates** (enabled by trade balance fluctuations) allow for **greater responsiveness of investment** to productivity shocks.

This affects **permanent income**:

\[
PI_t = r \cdot \left(b_t + \sum_{s=t}^{\infty} \frac{y_s - i_s}{(1 + r)^{s-t}} \right)
\]

If the interest rate responds less to domestic output (due to capital mobility), then investment increases more strongly after a positive productivity shock. That leads to a **larger permanent income effect**, increasing long-run consumption as well.

## 6. Policy Implications

- **Trade balance flexibility** allows countries to smooth interest rates independently of domestic shocks.
- **Investment smoothing** becomes possible in addition to consumption smoothing.
- This channel is especially important for **emerging markets**, where interest rate volatility is high and capital is scarce.
- Evaluations of international financial integration should incorporate the investment channel — otherwise, they understate welfare gains.

## 7. Conclusion

While the consumption risk sharing literature finds small gains from financial openness, that result hinges on the omission of investment dynamics. When countries can run trade imbalances, they stabilize domestic interest rates and allocate capital more effectively in response to shocks. This amplifies the effect of productivity on investment and permanent income, resulting in **significant welfare gains**. These gains go far beyond what consumption smoothing alone would suggest.


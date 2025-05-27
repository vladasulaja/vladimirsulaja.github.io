---
title: "Vladimir Sulaja – Personal Webpage"
layout: none
---

<link rel="stylesheet" href="style.css">
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async
        src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

<div class="navbar">
  <a href="#about">About</a>
  <a href="#projects">Projects</a>
  <a href="#contact">Contact</a>
</div>

<div class="container">

<img src="photo.jpg" alt="My Photo" style="border-radius: 8px; max-width: 120px; display: block; margin: 1rem auto;">

<h2 id="about">About Me</h2>

<p>
I hold a Ph.D. in Economics from the University of Zurich, where I conducted research in international finance, empirical banking, and the macroeconomic effects of automation. I am currently working as a Quantitative Analyst at BIT Capital.
</p>
<p>
This page features selected blog articles, models, and code that reflect my work and interests in global macroeconomics, finance, and econometrics.
</p>

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
  <h3>Emerging Market Business Cycles: The Story of Factor Shares</h3>
  <p>
    This paper investigates how differences in factor shares across countries contribute to the differences in business cycle properties between emerging market economies (EMEs) and developed economies (DEs). Using empirical data and standard international macroeconomic models, the paper shows that capital shares are strongly correlated with key moments such as the cyclicality of the trade balance and volatility of output. The findings suggest that differences in labor and capital income shares, observable across countries, play a central role in shaping macroeconomic dynamics.
  </p>
  <p><a href="projects/eme_bc.pdf" target="_blank">Read paper</a></p>
</div>

<div class="project">
  <h3>Investment, Interest Rates, and the Overlooked Gains from Trade Balance Flexibility</h3>

  <h4>Abstract</h4>
  <p>
    This article explores how a flexible trade balance can significantly enhance macroeconomic stability by delinking domestic interest rates from output shocks. We show that the standard consumption risk-sharing literature underestimates the welfare gains from international financial integration by neglecting the investment channel. Using a simple small open economy DSGE model, we demonstrate that allowing for a non-zero trade balance enables economies to better manage interest rate shocks, thus facilitating higher and less volatile investment in response to productivity shifts.
  </p>

  <h4>1. Introduction</h4>
  <p>
    The literature on international consumption risk sharing has long concluded that the welfare gains from global financial integration are small — often less than 1% of lifetime consumption. Seminal contributions by Lucas (1987), Obstfeld and Rogoff (1995), and more recently Gourinchas and Jeanne (2006) all show that the main benefit of risk sharing arises from the smoothing of consumption over time and states of nature.
  </p>
  <p>
    However, these studies typically neglect an important macroeconomic channel: <strong>investment amplification through interest rate smoothing</strong>. When the trade balance is restricted (e.g., balanced every period), countries must finance investment solely through domestic savings. This tight linkage between output, interest rates, and investment introduces fragility and lowers aggregate responsiveness to shocks.
  </p>
  <p>
    By contrast, when the <strong>trade balance can vary</strong>, countries can borrow during booms (or save during busts) to stabilize interest rates and <strong>optimize capital accumulation</strong>. This mechanism generates a second-round welfare gain that traditional consumption-only models miss.
  </p>

  <h4>2. The Model</h4>
  <ul>
    <li><strong>Cobb-Douglas production</strong>:<br>
      \( y_t = A_t \cdot k_{t-1}^\alpha \cdot l_t^{1-\alpha} \)
    </li>
    <li><strong>Capital accumulation</strong>:<br>
      \( k_t = (1 - \delta) \cdot k_{t-1} + i_t \)
    </li>
    <li><strong>Budget constraint</strong>:<br>
      \( c_t + i_t + r_t \cdot b_{t-1} = y_t \)
    </li>
    <li><strong>Debt evolution</strong>:<br>
      \( b_t = c_t + i_t + r_t \cdot b_{t-1} - y_t \)
    </li>
    <li><strong>Interest rate rule</strong>:<br>
      \( r_t = z_t + \phi_b \cdot b_{t-1} + \eta \cdot y_t \)
    </li>
  </ul>

  <h4>3. Dynare Implementation</h4>
  <pre><code>
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
</code></pre>

  <h4>5. Permanent Income and Risk Sharing</h4>
  <p>
    Standard models of international macroeconomics often evaluate the benefits of financial integration by computing the gains from consumption risk sharing. This is usually done by comparing expected utility under autarky (no financial integration) to that under perfect consumption insurance:
  </p>
  <p>
    \( \Delta U = E\left[\sum \beta^t \log(c_t^{\text{autarky}})\right] - E\left[\sum \beta^t \log(c_t^{\text{open}})\right] \)
  </p>
  <p>
    However, these models often abstract from the investment channel. They assume consumption is smoothed given an exogenous income process, but do not consider how smoother <strong>interest rates</strong> (enabled by trade balance fluctuations) allow for <strong>greater responsiveness of investment</strong> to productivity shocks.
  </p>
  <p>This affects <strong>permanent income</strong>:</p>
  <p>
    \( PI_t = r \cdot \left(b_t + \sum_{s=t}^{\infty} \frac{y_s - i_s}{(1 + r)^{s-t}} \right) \)
  </p>
  <p>
    If the interest rate responds less to domestic output (due to capital mobility), then investment increases more strongly after a positive productivity shock. That leads to a <strong>larger permanent income effect</strong>, increasing long-run consumption as well.
  </p>

  <h4>6. Policy Implications</h4>
  <ul>
    <li><strong>Trade balance flexibility</strong> allows countries to smooth interest rates independently of domestic shocks.</li>
    <li><strong>Investment smoothing</strong> becomes possible in addition to consumption smoothing.</li>
    <li>This channel is especially important for <strong>emerging markets</strong>, where interest rate volatility is high and capital is scarce.</li>
    <li>Evaluations of international financial integration should incorporate the investment channel — otherwise, they understate welfare gains.</li>
  </ul>

  <h4>7. Conclusion</h4>
  <p>
    While the consumption risk sharing literature finds small gains from financial openness, that result hinges on the omission of investment dynamics. When countries can run trade imbalances, they stabilize domestic interest rates and allocate capital more effectively in response to shocks. This amplifies the effect of productivity on investment and permanent income, resulting in <strong>significant welfare gains</strong>. These gains go far beyond what consumption smoothing alone would suggest.
  </p>
</div>

</div>

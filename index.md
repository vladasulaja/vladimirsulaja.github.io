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
  <a href="#shortpapers">Short Papers</a>
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

<h2 id="shortpapers">Short Papers</h2>

<div class="project">
  <h3>Investment, Interest Rates, and the Overlooked Gains from Trade Balance Flexibility</h3>
  <button onclick="toggleVisibility('shortpaper1')">Show/Hide Paper</button>

<div id="shortpaper1" style="display: none; margin-top: 1rem;">
  <h4>Idea</h4>
  <p>
    This short article explores how a flexible trade balance can significantly enhance macroeconomic stability by delinking domestic interest rates from output shocks. We show that the standard consumption risk-sharing literature underestimates the welfare gains from international financial integration by neglecting the investment channel. Using a simple small open economy DSGE model, we demonstrate that allowing for a non-zero trade balance enables economies to better manage interest rate shocks, thus facilitating higher and less volatile investment in response to productivity shifts.
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
      <li><strong>Technology shocks:</strong> \( a_t = \rho_a a_{t-1} + \varepsilon^a_t \)</li>
      <li><strong>Interest rate shock:</strong> \( z_t = \rho_z z_{t-1} + \varepsilon^z_t \)</li>
      <li><strong>Interest rate rule:</strong> \( r_t = z_t + \phi_b b_{t-1} + \eta y_t \)</li>
      <li><strong>Production:</strong> \( y_t = e^{a_t} \cdot k_{t-1}^{\alpha} \cdot l_t^{1 - \alpha} \)</li>
      <li><strong>Investment decision:</strong><br>
        \[
        i_t = \phi k_{t-1} \left[ \alpha e^{a_t} k_{t-1}^{\alpha - 1} l_t^{1 - \alpha} - r_t - \theta l_t \right] + \delta k_{t-1}
        \]
      </li>
      <li><strong>Capital accumulation:</strong> \( k_t = (1 - \delta) k_{t-1} + i_t \)</li>
      <li><strong>Labor-leisure condition:</strong> \( \phi_L l_t^{\psi} = (1 - \alpha) e^{a_t} k_{t-1}^\alpha l_t^{-\alpha} \)</li>
      <li><strong>Budget constraint:</strong> \( c_t + i_t + r_t b_{t-1} = y_t \)</li>
      <li><strong>Debt evolution:</strong> \( b_t = c_t + i_t + r_t b_{t-1} - y_t \)</li>
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


<h4>4. Mechanism: Interest Rate Smoothing and Permanent Income</h4>
<p>
  When interest rates rise with output, investment becomes expensive exactly when productivity is high. This distorts the optimal allocation of capital: firms and households want to invest, but are discouraged by high borrowing costs. Conversely, in downturns, interest rates fall but investment opportunities are limited. This procyclicality of interest rates leads to inefficient intertemporal allocation.
</p>

<p>
  A flexible trade balance, however, allows a country to <strong>decouple its domestic interest rate from its output</strong>. When domestic output is high, the country can run a trade deficit (import capital), preventing rates from rising too sharply. When output is low, it can run a surplus (export capital), keeping rates from falling too far.
</p>

<p>
  This <strong>interest rate smoothing</strong> leads to higher investment when it's most productive — during times of high productivity shocks. In turn, this amplifies the capital stock in future periods and raises the path of future income.
</p>

<p>
  Since <strong>permanent income</strong> is the present value of all future disposable income flows, a more elastic investment response leads to:
</p>

<p style="margin-left: 2rem;">
  \( PI_t = r \cdot \left(b_t + \sum_{s=t}^{\infty} \frac{y_s - i_s}{(1 + r)^{s-t}} \right) \)
</p>

<p>
  Higher output from more productive investment (and relatively stable consumption paths) thus raises permanent income, even if short-run consumption does not change much. This is the core reason why <strong>the welfare gains from financial openness are underestimated</strong> when the investment channel is ignored.
</p>

  <h4>5. Permanent Income and Risk Sharing</h4>
  <p>
    \( \Delta U = E\left[\sum \beta^t \log(c_t^{\text{autarky}})\right] - E\left[\sum \beta^t \log(c_t^{\text{open}})\right] \)
  </p>
  <p>
    \( PI_t = r \cdot \left(b_t + \sum_{s=t}^{\infty} \frac{y_s - i_s}{(1 + r)^{s-t}} \right) \)
  </p>

  <h4>6. Policy Implications</h4>
  <ul>
    <li>Trade balance flexibility allows countries to smooth interest rates independently of domestic shocks.</li>
    <li>Investment smoothing becomes possible in addition to consumption smoothing.</li>
    <li>This is especially important for emerging markets, where interest rate volatility is high and capital is scarce.</li>
    <li>Evaluations of international financial integration should incorporate the investment channel — or risk understating welfare gains.</li>
  </ul>

  <h4>7. Conclusion</h4>
  <p>
    While the consumption risk sharing literature finds small gains from financial openness, that result hinges on the omission of investment dynamics. When countries can run trade imbalances, they stabilize domestic interest rates and allocate capital more effectively in response to shocks. This amplifies the effect of productivity on investment and permanent income, resulting in significant welfare gains.
  </p>
</div>
</div>

</div>

<script>
  function toggleVisibility(id) {
    const el = document.getElementById(id);
    if (el.style.display === "none") {
      el.style.display = "block";
    } else {
      el.style.display = "none";
    }
  }
</script>


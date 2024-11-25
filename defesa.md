---
marp: true
paginate: true
---
<!-- _class: invert -->

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

# Multiresolution Spectral <br/>Representation of Neural Media

## Hallison Paz

### Dissertation Defense
#### IMPA, November 25th, 2024

![bg](assets/visgraf-background.jpeg)

<!-- _paginate: false -->

---

<!-- _class: invert -->

> # ... our goal is to derive a multiresolution representation of media objects using neural networks...


<!-- _footer: from "Motivation" section. -->
<!-- _paginate: false -->

---

# Representational Networks

- Continuous function
- Compact
- New methods/operations

![bg right](assets/cosine_approximation.gif)

<!-- _footer: Image: training of a ReLu MLP to fit a cosine wave -->

----

## Fitting a MLP using ReLu

![height:440px](assets/no-glasses.jpg)

![bg right height:500px](assets/masp.gif)

----

![bg](assets/paper-spectral-bias.png)

----

![](assets/paper-fourier-features.png)

<!-- _footer: Check [paper website](https://bmild.github.io/fourfeat/) -->

----

![](assets/paper-siren.png)

<!-- _footer: Check [paper website](https://www.vincentsitzmann.com/siren/) -->

----

## Fitting a sinusoidal MLP

![height:440px](assets/with-glasses.jpg)

![bg right height:500](assets/siren_masp.gif)

---

# Spectral Representation

![](assets/sine-decomposition.gif)

![bg left](assets/pink-floyd-prisma.jpg)

---

# Multiresolution

![](assets/multires-copper.png)

---

<!-- _class: invert -->

# Contributions

- **Comprehensive study on the initialization of sinusoidal neural networks**
<!-- - Family of neural architectures for encoding media objects at multiple resolutions
- Flexible framework for training multi-stage neural networks
- Application of our architecture in multiresolution imaging.
- Fourier Series-based initialization strategy for periodic functions
- technique based on the Poisson Equation to generate seamless material textures.
- our architecture can be integrated into the rendering pipeline of textured objects. -->

<!-- _footer: Based on dissertation's page 5 -->

---
<!-- _backgroundColor: #003463-->
<!-- _class: invert -->
<!-- _paginate: false -->

# Frequency Dynamics in Sinusoidal Neural Networks

---

# Shallow Network

###### A perceptron:
$y = \sin(Wx + b)$

###### A shalow network:
$f(x) = a_0 + \sum_{j=1}^m a_j \sin\left(\omega_j x + \varphi_j\right)$


![bg right fit](img/ch4/pure-sine.png)

---

# Shallow Network

![](assets/low-freqs-32.png)


<!-- _footer: Figure 3.2 Reconstruction of a signal with four distinct frequencies, where the lower frequencies <br/> fall within  the initialization range of the network’s first layer. -->

---

![](assets/high-freq-42.png)

<!-- _footer: Figure 3.3: Reconstruction of a signal with four distinct frequencies, where only the higher frequencies <br/> fall within the initialization range  of the network’s first layer. -->

---

### Capacity matters

![h:520px](assets/capacity-matters.png)

<!-- _footer: Figure 3.4: Reconstruction of the signal using networks with different width, all initialized with frequencies in [−45, 45] Hz. -->

---

> ...a shallow network with only one layer of sinusoidal activation functions can filter a signal by band-limiting its frequency content...

<!-- _footer: From page 28. -->

---

![bg fit](assets/sinusoidal-layer-meme.png)

<!-- _paginate: false -->

---

## Composition of sines generates much more frequencies

![](assets/generated_frequencies.png)


----
<!-- 
<style>
        body {
            margin: 0;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .vertical-line {
            width: 2px; /* Thickness of the line */
            height: 100%; /* Full height of the page */
            background-color: black; /* Color of the line */
        }
    </style> -->


<!-- # 1 Hidden Layer - low frequencies -->

![bg fit](assets/thesis-hidden-layer-captures-low.png)

![bg fit](assets/thesis-hidden-layer-captures-high.png)

<!-- _footer: TOP: single hidden layer; <br/> LEFT (Figure 3.6): lower frequencies initialization; RIGHT (Figure 3.7): higher frequencies initialization. <br> -->

<!-- _paginate: false -->

---

<!-- _backgroundColor: #003463-->
<!-- _class: invert -->
<!-- _paginate: false -->

# Stochastic Signals

---

# Perlin Noise

![](assets/smoothed-noise-1hl.png)

<!-- _footer: Figure 3.12: Smoothed reconstruction with 1 hidden layer; initialization: 10Hz. -->

---

## By the way...

![height:500px drop-shadow center](assets/foto-perlin.jpg)

<!-- _paginate: false -->

<!-- _footer: Met professor Ken Perlin at SIGGRAPH'24 -->

---

![h:600 center](assets/varying-frequencies.png)

<!-- _footer: 32 neurons per layer; 1 hidden layer. -->

---

![h:600 center](assets/varying-width.png)

<!-- _footer: 2Hz initialization; 1 hidden layer. -->

---

### Training, generalization and representation

![h:500 center](assets/generalization.png)

<!-- _footer: From figure 3.20. -->

---

<!-- _class: invert -->

# Contributions

- Comprehensive study on the initialization of sinusoidal neural networks
- **Family of neural architectures for encoding media objects at multiple resolutions**
- **Flexible framework for training multi-stage neural networks**
<!-- - Application of our architecture in multiresolution imaging.
- Fourier Series-based initialization strategy for periodic functions
- technique based on the Poisson Equation to generate seamless material textures.
- our architecture can be integrated into the rendering pipeline of textured objects. -->

<!-- _footer: Based on dissertation's page 5 -->

---

<!-- _backgroundColor: #003463-->
<!-- _class: invert -->
<!-- _paginate: false -->

# Multiresolution Sinusoidal Neural Networks

---

# Multiscale Decomposition

- Let $\mathscr{f}:\mathcal{D}\to \mathcal{C}$ be a *ground-truth signal*

- We decompose it into a sum of $N$ stages: 

$$\mathscr{f}=\mathscr{g}_0 + \dots + \mathscr{g}_{N-1}$$
, 
<!-- where $\gt{g}_0$ captures the coarsest approximation of the signal and $\gt{g}_i$, for $i>0$, progressively introduces higher-frequency components.  -->
- $\mathscr{g}_0$ represents the coarsest features

---

# Multiscale decomposition

The *level of detail* at stage $i$ is defined as:

$$
\mathscr{f}_i = \mathscr{g}_0 + \cdots + \mathscr{g}_i \quad \text{or} \quad \mathscr{f}_i = \mathscr{f} - \sum_{j=i+1}^{N-1} \mathscr{g}_j.
$$

Each stage $\mathscr{g}_i$ is computed as:
$$
\mathscr{g}_i = \mathscr{f}_{i+1} - K * \mathscr{f}_{i+1}, \quad \text{where } \mathscr{f}_{N-1} = \mathscr{f}.
$$

---

Example: TOP: $\mathscr{g}_i$ | Bottom: $\mathscr{f}_i$

![](assets/details.png)

![](assets/filtered-incremental.png)

<!-- _backgroundColor: #000000-->

<!-- _class: invert -->

---


**Contrast enhanced**: TOP: $\mathscr{g}_i$ | Bottom: $\mathscr{f}_i$

<p align="right">
  <img src="assets/details-v2.png" alt="S-Net architecture" style="height:230px;"/>
</p>

![](assets/filtered-incremental.png)

<!-- _backgroundColor: #000000-->

<!-- _class: invert -->

---

## Multiresolution [Sinusoidal] Neural Networks (MR-Net)

<br/>

$$f:\mathcal{D} \times [0,N] \to \mathcal{C}$$
<br/>

$$f(x,t) = c_0(t) g_0(x) + \cdots + c_{N-1}(t) g_{N-1}(x)
$$
<br/>

$c_i(t) = \max \Big\{ 0, \min \big\{ 1, t - i \big\} \Big\}.$

![bg right fit](assets/mr-net-stages-v2.png)

---

# MR-Module
<br/>
<br/>

![](assets/diagram_mr_module.jpg)


---

# Shallow Network (S-Net)

<br/>

<!-- <p align="center">

![h:350](img/snet.jpg)

</p> -->

<p align="center">
  <img src="assets/snet.jpg" alt="S-Net architecture" style="height:350px;"/>
</p>

---

# Laplacian Network (L-Net)

<br/>

<p align="center">
  <img src="assets/lnet.jpg" alt="S-Net architecture" style="height:350px;"/>
</p>

---

# Modulated Network (M-Net)

<br/>

<p align="center">
  <img src="assets/mnet.jpg" alt="S-Net architecture" style="height:350px;"/>
</p>


---

# Multiresolution Training

![bg left:60% fit](assets/mr-training-algorithm.png)


---

# Training elements

### Frequency Initialization

![width:500px](img/ch4/nyquist.png)

### Loss function

$$\mathcal{L}_i(\theta_i)=\frac{1}{K_i}\sum ||f_i(x_j)-y_j||^2.$$

![bg right fit](assets/input-data.png)

---

<!-- _backgroundColor: #003463-->
<!-- _class: invert -->
<!-- _paginate: false -->

# Multiresolution Imaging


---

<!-- _class: invert -->

# Contributions

- Comprehensive study on the initialization of sinusoidal neural networks
- Family of neural architectures for encoding media objects at multiple resolutions
- Flexible framework for training multi-stage neural networks
- **Application of our architecture in multiresolution imaging.**
<!-- - Fourier Series-based initialization strategy for periodic functions
- technique based on the Poisson Equation to generate seamless material textures.
- our architecture can be integrated into the rendering pipeline of textured objects. -->

<!-- _footer: Based on dissertation's page 5 -->

---

# M-Net is more compact

![](assets/mrnet-variants-table.png)

---

# Multiresolution image representation



![h:500 center](assets/mnet-cameraman.png)

---

# More natural than Bacon



![center height:500px](assets/bacon-cameraman.png)

<!-- ![bg fit](assets/mnet-spectra.png) -->

---

# Comparable to Siren

![h:500 center](assets/mnet-vs-siren.png)

---

# 7 levels of Multiresolution with same size

![](assets/mnet-comparison-table.png)

---

# Texture magnification / minification

![h:500 center](assets/mr-tapete.png)

---

# Continuous Scale


<video width="1200" height="800" controls>
  <source src="assets/multiresolution-masp.mp4" type="video/mp4">
</video>

---

# Anti-aliasing

![h:500px center](assets/mr-antialiasing.png)

---

<!-- _class: invert -->

# Contributions

- Comprehensive study on the initialization of sinusoidal neural networks
- Family of neural architectures for encoding media objects at multiple resolutions
- Flexible framework for training multi-stage neural networks
- Application of our architecture in multiresolution imaging.
- **Fourier Series-based initialization strategy for periodic functions**
<!-- - **Demo of architecture integration into the rendering pipeline of textured objects.** -->
<!-- - Technique based on the Poisson Equation to generate seamless material textures. -->

---

<!-- _backgroundColor: #003463-->
<!-- _class: invert -->
<!-- _paginate: false -->

# Periodic Textures

---

## Siren extrapolation

<!-- _class: invert -->

<br/>

![h:300](img/ch6/leopard-train-data.png)

![bg right fit](img/ch6/siren_extrapolation.png)

---

# Periodic Image

$\mathscr{f}:\mathbb{R}^2\to \mathbb{R}$,  $\mathscr{f}(x) \!=\! \mathscr{f}(x + P)$  with $P=(P_1, P_2)\in \mathbb{R}^2$ 

### Shallow sinusoidal networks

$$f(x) = L\circ S(x) =  c_0 + \sum_{i=1}^{n} c_i  \sin\Big(\langle{\omega_i}, { x}\rangle+ \varphi_i\Big)$$

- if $\omega_i = \frac{2k\pi}{P}$, $k \in \mathbb{Z}$, then a shallow network is periodic. 

###### What about a deep sinusoidal network?

---

# Periodic Neural Networks

$$f(x) = L\circ H \circ \,S(x),$$
$$H\circ S(x):=\sin\big(W \cdot S(x)+b\big)$$

$$h_{i}\circ s(x) = \sin\Big(\sum_{j=1}^{n} W_{ij}\sin\Big(\langle{\omega_j}, {x}\rangle +\varphi_j\Big) + b_{i}\Big)$$

<br/>

From Novello [2022]:

$$h(x)= \!\!\sum_{\textbf{l}\in\mathbb{Z}^n}\left[\prod_{i=1}^n J_{l_i}(a_i)\right]\sin\Big(\langle{\textbf{l}}, {\omega x +\varphi}\rangle+ b\Big)$$

---
# Periodic Neural Networks

### Theorem
>If the first layer of a sinusoidal MLP $f$ is periodic with period $P$, then $f$ is also periodic with period $P$.

<!-- _class: invert -->

---

# Frequency Initialization


$$\sin(\omega_i x + \varphi_i) = \cos(\varphi_i) \sin(\omega_i x) - \sin(\varphi_i)\sin\left(-\omega_i x + \frac{\pi}{2}\right)  $$

> ...we only need to sample the integer values $k_j$ from the half-square $\textbf{K} = [0, B] \times [-B, B]$ 

---

# Frequency Initialization

<!-- ![](img/ch6/leopard_chosen_frequencies.png)
![](img/ch6/leopard_generated_frequencies.png)
![](img/ch6/mnet_extrapolation.png)
![](img/ch6/leopard-train-data.png) -->

<table>
  <tr>
    <td> <img src="img/ch6/leopard_chosen_frequencies.png"  alt="1" > <p align="center">(A)</p> </td>
    <td><img src="img/ch6/leopard_generated_frequencies.png" alt="2"><p align="center">(B)</p></td>
    <td><img src="img/ch6/mnet_extrapolation.png" alt="3" width="512"><p align="center">(C)</p></td>
    <td><img src="img/ch6/leopard-train-data.png" alt="4"><p align="center">(D)</p></td>
   </tr> 
</table>

<!-- _footer: Figure 6.2: (A) Chosen frequencies for initialization; (B) Fourier transform of the trained model; <br/> (C) Seamless reconstruction in $[−2,2]^2$; (D) Training data in $[−1,1]^2$ -->

---

<!-- _backgroundColor: #000000 -->
<video width="1200" height="700" controls>
  <source src="assets/periodicbrown.mov" type="video/mp4">
</video>

---

<!-- _backgroundColor: #000000 -->
<video width="1200" height="700" controls>
  <source src="assets/periodicmoss.mov" type="video/mp4">
</video>

---

# Samples can be given anywhere

![height:500px center](assets/tile-equivalence.png)

---

![](img/ch6/diagram.png)

<!-- _footer: Multiresolution using M-Net -->

---

<!-- _class: invert -->

# Contributions

- Comprehensive study on the initialization of sinusoidal neural networks
- Family of neural architectures for encoding media objects at multiple resolutions
- Flexible framework for training multi-stage neural networks
- Application of our architecture in multiresolution imaging.
- Fourier Series-based initialization strategy for periodic functions
- **Demo of architecture integration into the rendering pipeline of textured objects.**
<!-- - **Technique based on the Poisson Equation to generate seamless material textures.** -->

---
<!-- _backgroundColor: #000000 -->
<video width="1200" height="700" controls>
  <source src="assets/torus_dirty.mov" type="video/mp4">
</video>

---
<!-- _backgroundColor: #000000 -->
<video width="1200" height="700" controls>
  <source src="assets/toro-marrow.mov" type="video/mp4">
</video>

---

<!-- _backgroundColor: #003463-->
<!-- _class: invert -->
<!-- _paginate: false -->

# Seamless Textures

---

# Contributions

- Comprehensive study on the initialization of sinusoidal neural networks
- Family of neural architectures for encoding media objects at multiple resolutions
- Flexible framework for training multi-stage neural networks
- Application of our architecture in multiresolution imaging.
- Fourier Series-based initialization strategy for periodic functions
- Demo of architecture integration into the rendering pipeline of textured objects.
- **Technique based on the Poisson Equation to generate seamless material textures.**

---

# Poisson regularization

$$\mathscr{L}(\theta)= {\int_{\Omega} \lambda\big(\mathscr{f}-f\big)^2dx} + {\int_{\Omega} (1-\lambda)||{{J}({f})-U}||^2dx}$$

![height:320 center](img/ch6/gradients-merged.png)

<!-- _footer: should correct at page 92. -->

---

![bg](img/ch6/extrapolation_jeans.png)

---

![height:600 center drop-shadow](assets/seamless-reconstruction.png)

---

<!-- _backgroundColor: #003463-->
<!-- _class: invert -->
<!-- _paginate: false -->

# Conclusion

---

# Thanks

### Reach out: hallpaz@impa.br
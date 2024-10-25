---
layout: post
title:  "ILE Matrices II"
date:   2024-09-24 
categories: jekyll update
---

Crazy how much of a difference an inverse makes :)

In a previous post I described a researcher's construction for matrices with integer-linear eigenvalues. Each matrix entry is $\phi_{j}\phi_{i}^{-1}$, which *does* in fact ensure integer-linearity for any partial order. Despite that initial error I was lucky enough to explore partial orders whose permutations were self-inverses (and have some other benefits), allowing me to still study these objects. 

Recently I finished a project with a good friend (check out his [website](https://www.abemill.com/projects.html)!), and we wrote some [scripts](https://github.com/symeig) we are excited about. A common motif in computational sciences is to design algorithms around specific structures (e.g. with matrices - tridiagonal, symmetric, sparse, etc.) to maximally leverage it, and that is exactly what we did with integer-linear guarantees on eigenvalue structure. The main algorithm:


$\text{main\(}A\text{, batchsize, stagger\): }$  
&nbsp;&nbsp;&nbsp;&nbsp; $n\text{, midpoint, midpointvalue, symbols} := \dots$  
&nbsp;&nbsp;&nbsp;&nbsp; $\text{digits} := \text{batchsize}\*\(\text{stagger}+1\)$  
&nbsp;&nbsp;&nbsp;&nbsp; $\text{indicatorvars} := \[\text{base}^{\lceil \frac{\text{digits}}{2} \rceil - \text{stagger}}, \\; \text{base}^{\lceil \frac{\text{digits}}{2} \rceil - 2 * \text{stagger}}, \\; \dots, \\; \text{base}^{-\lfloor \frac{\text{digits}}{2} \rfloor}\]*i$  
&nbsp;&nbsp;&nbsp;&nbsp; $\text{map} := \\{\text{symbols } \: \text{ randomreals}\\}$  
&nbsp;&nbsp;&nbsp;&nbsp; $\text{coeffs} := \[\]$  
  
&nbsp;&nbsp;&nbsp;&nbsp; $\text{for }\(i=1 \text{ to }\lceil \frac{n}{\text{batchsize}} \rceil\):$  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $\text{mapcurr} = \text{map.copy}$  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $\text{mapcurr}\[i*\text{batchsize}, \text{ min}\(n, \\; \(i+1\)\*\text{batchsize}\)\] \text{ += indicatorvars}$  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $A_{i} = \text{mapcurr}(A)$  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $\sigma = eig(A_{i}) + \text{midpoint}$  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $\text{coeffs.insert}\(\[\text{digits}\(\sigma_{j}\) - \text{midpointvalue for } j \text{ in range}\(n\)\]\)$  
  
&nbsp;&nbsp;&nbsp;&nbsp; $\text{return } \[vec\(\text{coeffs}\[:,0\]\), \\; \dots, \\; vec\(\text{coeffs}\[:,n-1\]\)\]$  

Symbolic variables are encoded as complex numbers, fixing one complex part to preserve eigenvalue ordering across batches, and the other dynamically as unique power terms to recover their identities.

It is rare for me to truly believe in a project, but in this case I am chugging the koolaid. I concur with the researchers who discovered these matrices that it does compare to a miracle, and miracles beget miracles. That feeling pushed me to work - the speedups we achieved are only a trace of the potential energy of these objects. Also - check out this sweet triangle. 

<p align="center">
  <img src="/assets/images/0702t.png" width="300"/>
</p>
<p align=center>
<sub> Projective barycentric coordinate for a conservative $n=3$ system where $a+b+c=1$. Allows negative masses and extension to infinity (points marked with $\pm$ are $\pm \infty$ in a given variable for $abc$). </sub>
</p>

Advocating for yourself in this cyberpunk dystopia we're barreling towards can be exhausting when few others do. You may encounter professionals who are disingenuous, treat students as workhorses, or work against their best interests. If you are going through something similar, trust your gut and work towards independence. Sometimes your RNG is busted and you're overdue - but at the end of the day you gotta sample the loot table distribution for good drops. And farm elsewhere if it seems promising.

I'd like to go into more detail about the matrix generation scripts work because they are really cool, but will save it for another time. Thanks for reading!

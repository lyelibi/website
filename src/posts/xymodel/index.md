---
title: "Topological Phase Transitions: The XY Model and Vortex Unbinding"
date: 2023-10-31
tags: ["physics", "simulation", "python", "topology", "nobel-prize"]
description: "Moving beyond discrete spins to explore continuous symmetry, topological defects, and the Kosterlitz-Thouless transition."
layout: layouts/post.njk
image: "/assets/images/xymodel/xy.webp"
---

In our previous explorations of statistical mechanics, we looked at systems with discrete symmetries: the [Ising Model](/posts/ising-model) (up/down spins) and the [Potts Model](/posts/potts-model) ($q$ distinct colors). In both cases, phase transitions occurred when thermal noise overcame the energy benefit of neighboring spins aligning into one of these discrete states.

But what happens if we remove the discrete constraint? What if spins can point in *any* direction in a plane?

This brings us to the **XY Model**, a system that introduces continuous symmetry and forces us to confront a profoundly different kind of ordering mechanism based on **topology**.

## The Move to Continuous Symmetry

In the XY model, spins are no longer simple integers. They are 2D unit vectors constrained to rotate in a plane. The state of site $i$ is defined by an angle $\theta_i \in [0, 2\pi)$.

The ferromagnetic interaction energy tries to minimize the angle between neighbors:

$$
H = -J \sum_{\langle i,j \rangle} \mathbf{s}_i \cdot \mathbf{s}_j = -J \sum_{\langle i,j \rangle} \cos(\theta_i - \theta_j)
$$

This introduction of continuous $O(2)$ symmetry creates a theoretical problem. The **Mermin-Wagner theorem** states that in two dimensions, continuous symmetries cannot break spontaneously. Thermal fluctuations (spin waves) are strong enough to destroy true long-range ferromagnetic order at *any* non-zero temperature.

According to standard theory, the 2D XY model should never order. Yet, simulations clearly show a phase transition.

## Enter Topology: Vortices

The resolution to this paradox lies in **topological defects**. While spins cannot align perfectly over long distances, the system can develop "knots" in the spin field that cannot be smoothed out smoothly.

These defects are called **vortices**.

A vortex is a point around which the spin vectors rotate by exactly $2\pi$ (a full circle).
* If they rotate counter-clockwise, it's a **+1 Vortex**.
* If they rotate clockwise, it's a **-1 Antivortex**.

The phase transition in the XY model is not a competition between ordered spins and random flips. It is a competition between bound vortex pairs and a free vortex plasma.

## The Kosterlitz-Thouless (KT) Transition

This unique transition mechanism earned Kosterlitz, Thouless, and Haldane the 2016 Nobel Prize in Physics.

1.  **Low Temperature (Quasi-Ordered Phase):** It costs a lot of energy to create a lone vortex. However, it is energetically cheap to create a tight **vortex-antivortex pair**. Viewed from a distance, the +1 and -1 charges cancel out, leaving the background spin field relatively smooth (exhibiting "quasi-long-range order").
2.  **High Temperature (Disordered Phase):** As temperature increases, entropy dominates. The thermal energy becomes sufficient to rip these pairs apart. The vortices "unbind" and move freely through the lattice like a plasma, scattering spin correlations and destroying order completely.

## Simulation Results

We simulated the XY model using Metropolis dynamics and explicitly tracked topological defects by calculating the winding number around every plaquette on the lattice.

<div class="full-bleed" style="--full-bleed-max:60vw; margin:0 auto;">
  <img src="/assets/images/xymodel/xy.webp" alt="Dashboard showing the XY model phase transition. Top row visualizes spin angles and vortex locations at different temperatures. Bottom row shows magnetization and vortex density curves." style="width:100%; height:auto;">
  <figcaption style="text-align:center; color:#666; font-style:italic;">Fig 1. The Kosterlitz-Thouless Transition. The background colors represent spin angles. White dots are vortices (+1), black dots are antivortices (-1).</figcaption>
</div>

### Visualizing the Fields (Top Row)

The snapshots in the top row perfectly illustrate the unbinding mechanism:

* **High T (Left):** The system is a disordered plasma. The background colors (angles) are random noise. Crucially, the image is covered in scattered white and black dots—these are free, unbound vortices roaming the lattice.
* **Near $T_{KT}$ (Middle):** We are near the critical point. The number of free vortices has dropped significantly, and we begin to see larger patches of smoothly changing color.
* **Low T (Right):** The topological "cleanup" is complete. The lattice is almost entirely free of dots. Any defects that exist are locked in tight, neutral pairs (too small to see easily at this scale). The background shows smooth, wave-like gradients, characteristic of the quasi-ordered phase dominated by spin waves rather than defects.

### The Quantitative Signal (Bottom Row)

* **Vortex Density (Bottom Right):** This plot is the signature of the KT transition. At low temperatures, the density of free vortices is effectively zero. As $T$ crosses the critical threshold (dashed line), the density does not jump discontinuously; instead, it rises exponentially as thermal energy rapidly unbinds the pairs.
* **Magnetization (Bottom Left):** While true infinite-range order is forbidden, in a finite simulation box, the correlation length can exceed the system size. The rise in magnetization here indicates the onset of quasi-long-range order where spin waves become the dominant fluctuation mode.

## Conclusion

The XY model demonstrates that phase transitions can be driven by more than just local symmetry breaking. By moving to continuous spins, we uncovered a transition driven by global topological topology—the unbinding of vortex defects. This concept has since become fundamental in understanding phenomena ranging from superfluid helium to exotic states of quantum matter.
---
title: "Super-Paramagnetic Clustering (SPC)"
summary: "Clustering without Parameters: The Super-Paramagnetic Potts Model"
authors: []
tags: ["machine-learning", "physics", "clustering", "python"]
categories: [Computational Physics]
date: 2026-02-17T00:00:00Z
featured: true
draft: false

# Enable math rendering (KaTeX/MathJax)
math: true
# Optional: Featured image setup
image:
  caption: "Super-Paramagnetic Clustering (SPC)"
  focal_point: "Smart"
  preview_only: false
  path: "/assets/images/pottsmodel/potts_spc_circles.webp"

design:
  # Choose how many columns the section has. Valid values: '1' or '2'.
  columns: '2'
---

In our previous posts, we explored the [Ising Model](/posts/ising-model) and the [$q$-state Potts Model](/posts/potts-model) on fixed 2D lattices. We observed how these systems undergo phase transitions, organizing themselves from chaotic noise into ordered domains as the temperature drops.

Today, we take a leap from pure physics into machine learning. What happens if we replace the rigid lattice with a graph derived from data?

This leads us to **Super-Paramagnetic Clustering (SPC)**, a method that uses the thermodynamics of the Potts model to naturally discover structures in data without needing to specify the number of clusters beforehand.

## From Lattices to Data Graphs

In the standard Potts model, spins interact only with their fixed neighbors on a grid, and the interaction strength $J$ is constant. In SPC, we treat data points $x_i$ as nodes in a graph. We build edges using a nearest-neighbor approach (like K-NN) or a topological approach (like **Delaunay Triangulation**).

Crucially, the interaction strength is no longer constant. It becomes a variable $J_{ij}$ that decays with the distance between points:

$$
J_{ij} = \exp\left(-\frac{||x_i - x_j||^2}{2 \bar{d}^2}\right)
$$

This creates a physical system where "friends" (nearby points) have strong bonds, while "strangers" (distant points) have weak bonds.

## The Three Phases of Data

When we simulate this system using the **Swendsen-Wang algorithm**, we observe three distinct phases as we sweep the temperature $T$:

1.  **Ferromagnetic Phase (Low T):** The system is frozen. All bonds are active, and the entire dataset looks like one giant cluster. (Useful only for outlier detection).
2.  **Paramagnetic Phase (High T):** The system is melted. Thermal noise breaks all bonds, and every data point becomes its own tiny cluster.
3.  **Super-Paramagnetic Phase (Intermediate T):** This is the sweet spot. The thermal energy is high enough to break the **weak links** between natural groups, but low enough that the **strong links** within groups remain intact.

In this phase, the "clusters" identified by the Swendsen-Wang algorithm correspond exactly to the natural clusters in your data.

## Results: Solving Topological Problems

We tested this method on two challenging toy datasets. The dashboards below show the cluster evolution (top row) and the thermodynamic observables (bottom row).

### Case 1: Concentric Circles (Topology)

Standard algorithms like K-Means assume clusters are globular "blobs" and fail spectacularly on rings, usually slicing them in half.

<div class="full-bleed" style="--full-bleed-max:60vw; margin:0 auto;">
  <img src="/assets/images/pottsmodel/potts_spc_circles.webp" alt="SPC applied to concentric circles showing perfect separation of rings." style="width:100%; height:auto;">
  <figcaption style="text-align:center; color:#666; font-style:italic;">Fig 1. Topological clustering of concentric circles. Note the peak in Susceptibility ($\chi$) which identifies the optimal temperature.</figcaption>
</div>

**The Result:** At the critical temperature ($T \approx 0.18$), the model successfully separates the inner ring from the outer ring. The algorithm propagates the "spin information" through the dense chains of data points but refuses to jump the low-density gap between the rings.

**The Signal:** Look at the **Susceptibility ($\chi$)** plot in the bottom center. The massive peak tells us exactly where the phase transition occurs. In an unsupervised setting where we don't know the answer, this peak acts as a signal flare saying, "Look here! The structure is most stable at this temperature."

### Case 2: High-Dimensional Blobs

We also generated clusters in 10-dimensional space and projected them to 2D for graph construction (using PCA-based Delaunay triangulation).

<div class="full-bleed" style="--full-bleed-max:60vw; margin:0 auto;">
  <img src="/assets/images/pottsmodel/potts_spc_blobs.webp" alt="SPC applied to 10D blobs showing clean recovery of 3 clusters." style="width:100%; height:auto;">
  <figcaption style="text-align:center; color:#666; font-style:italic;">Fig 2. Clustering of high-dimensional Gaussian blobs. The paramagnetic 'gray' points represent noise that the model naturally filters out.</figcaption>
</div>

**The Result:** The method cleanly recovers the three distinct groups. Notably, the gray points in the snapshots represent "paramagnetic" nodes—points that fluctuate too wildly to belong to any stable core. This highlights a hidden feature of SPC: it performs **automatic outlier detection**, isolating noise points that don't fit well into any cluster.

## Conclusion

By adapting the Potts model to data-derived graphs, we transform a statistical mechanics simulation into a robust clustering algorithm. It trades the difficulty of choosing '$k$' (number of clusters) for the physics of choosing '$T$' (temperature). However, thanks to the susceptibility peak, the physics gives us a clear metric to find the optimal solution, making SPC a powerful tool for discovering complex, non-linear structures in data.
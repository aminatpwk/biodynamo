
**Research Fork** — This repository is a fork of [BioDynaMo](https://github.com/BioDynaMo/biodynamo) maintained for the research project below. The original README follows.

[![Language: C++](https://img.shields.io/badge/language-C%2B%2B-00599C?logo=c%2B%2B&logoColor=white)](https://en.wikipedia.org/wiki/C%2B%2B) [![Build: CMake](https://img.shields.io/badge/build-CMake-6A6A6A?logo=cmake&logoColor=white)](https://cmake.org/) [![License: Apache-2.0](https://img.shields.io/badge/license-Apache%202.0-blue)](./LICENSE) [![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20macOS-lightgrey)]() 

---

## Understanding Performance Bottlenecks of Large-Scale Agent-Based Simulations

This fork supports research into the computational performance characteristics of large-scale
agent-based simulations built on BioDynaMo, with a focus on memory-bound workload behavior
and hardware-level profiling.

### Research Focus

- Identifying and characterizing performance bottlenecks (LLC misses, memory bandwidth saturation)
  in biologically realistic agent-based simulations
- Profiling with hardware performance counters (`perf`) against established memory-boundedness
  thresholds (DAMOV benchmark suite)
- Evaluating the suitability of Processing-In-Memory (PIM) architectures for offloading
  memory-bound simulation kernels

### Simulation Workload

The primary workload used in this study is the SIR agent-based epidemiology demo located in
`demo/epidemiology`. The demo implements a stochastic SIR (Susceptible–Infected–Recovered)
model with the following characteristics:

- Agents (`Person`) hold a discrete state: `kSusceptible`, `kInfected`, or `kRecovered`.
- Infection is implemented via an `Infection` behavior: when a susceptible agent is considered,
  it draws a random value and, if below the per-step `infection_probability`, performs a
  neighbor query within `infection_radius` (via `UniformGridEnvironment::ForEachNeighbor`) to
  detect infected neighbors; if any infected neighbor is found the agent becomes infected.
- Recovery is modeled via a `Recovery` behavior that applies a per-step `recovery_probability`
  to transition infected agents to recovered.
- Agents may also perform `RandomMovement`, producing irregular spatial updates and
  scatter-access memory patterns during neighbor lookups.

These neighbor queries and irregular agent interactions create memory access patterns
that are suitable for studying memory-boundedness, cache behavior, and the impact of
hardware-level optimizations.

Key parameters (see `demo/epidemiology/src/sim-param.h`) include `beta`, `gamma`, `infection_radius`,
`infection_probability`, `recovery_probability`, `initial_population_*`, and `number_of_iterations`.

### Affiliation

Amina Sokoli — Secure and Sustainable System Scaling Lab, CISPA Helmholtz Center for Information Security  
Advisor: Abdullah Giray Yağlıkçı

---

The original README follows.

## What is BioDynaMo?

BioDynaMo is a software platform to easily create, run, and visualise 3D agent-based simulations.
* **Blazing fast:** The core of the platform is written in C++ and is highly optimized to harness the computational power of modern hardware.
* **Modular:** Reuse, adapt, or create modules that represents a specific biological behavior or entity. With BioDynaMo you don't need to start from scratch anymore!
* **Visual:** Effortlessly create astonishing images and animations of your simulations. Your publications and presentations will stand out even more!

## Installation

Simply run the following command:

```
curl https://biodynamo.github.io/install | bash
```

For more information, check out our [user's guide](https://www.biodynamo.org/user-guide) documentation that is designed for people who will use BioDynaMo on a day-to-day basis, and aims at providing users with all essential info to make full use of BioDynaMo.

<!-- ## Examples
-- Show some nice visualizations here, with a one-liner explanation -->

## Contributing

The main purpose of this repository is to evolve BioDynaMo, making it faster and easier to use. Development of BioDynaMo happens in the open on Github, and we are grateful to the community for contributing bugfixes and improvements. Please read our [contribution guide](https://github.com/BioDynaMo/biodynamo/blob/master/doc/dev_guide/contribute.md) and learn about our development process, how to propose bugfixes and improvements, and how to build and test your changes to BioDynaMo.

We pledge to act and interact in ways that contribute to an open, welcoming, diverse, inclusive, and healthy community in BioDynaMo. Thus, we expect all contributors to adhere to our [covenant for contributors](https://www.biodynamo.org/developer-guide/contributor-covenant).

## Citation

When publishing work that relies on BioDynaMo, please cite the following manuscripts, which underwent rigorous peer review. 
Citations are crucial to improve your work's reproducibility, show funding agencies that our work is valuable to the community, and increase your peers' knowledge about our terrific tool.

> Lukas Breitwieser, Ahmad Hesam, Jean de Montigny, Vasileios Vavourakis, Alexandros Iosif, Jack Jennings, Marcus Kaiser, Marco Manca, Alberto Di Meglio, Zaid Al-Ars, Fons Rademakers, Onur Mutlu, Roman Bauer, 
> **BioDynaMo: a modular platform for high-performance agent-based simulation**, Bioinformatics, Volume 38, Issue 2, January 2022, Pages 453–460, https://doi.org/10.1093/bioinformatics/btab649

```
@article{breitwieser_biodynamo_2022,
    author = {Breitwieser, Lukas and Hesam, Ahmad and de Montigny, Jean and Vavourakis, Vasileios and Iosif, Alexandros and Jennings, Jack and Kaiser, Marcus and Manca, Marco and Di Meglio, Alberto and Al-Ars, Zaid and Rademakers, Fons and Mutlu, Onur and Bauer, Roman},
    title = "{BioDynaMo: a modular platform for high-performance agent-based simulation}",
    journal = {Bioinformatics},
    volume = {38},
    number = {2},
    pages = {453-460},
    year = {2021},
    month = {09},
    issn = {1367-4803},
    doi = {10.1093/bioinformatics/btab649},
    url = {https://doi.org/10.1093/bioinformatics/btab649}
}
```
<blockquote>

Lukas Breitwieser, Ahmad Hesam, Fons Rademakers, Juan Gómez Luna, and Onur Mutlu. 2023. 
 **High-Performance and Scalable Agent-Based Simulation with BioDynaMo.**
 In Proceedings of the 28th ACM SIGPLAN Annual Symposium on Principles and Practice of Parallel Programming (Montreal, QC, Canada) (PPoPP ’23). 
 Association for Computing Machinery, New York, NY, USA, 174–188. https://doi.org/10.1145/3572848.3577480 arXiv:[2301.06984](https://arxiv.org/abs/2301.06984) [cs.DC]

<div>
<a href="https://blog.biodynamo.org/2023/04/PPoPP23-best-artifact-biodynamo.html"><img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/trophy.svg" width="25" height="25" /></a>
Received <a href="https://blog.biodynamo.org/2023/04/PPoPP23-best-artifact-biodynamo.html" >Best Artifact Award</a>.
</div>
</blockquote>

```
@inproceedings{breitwieser_biodynamo_2023,
  author = {Breitwieser, Lukas and Hesam, Ahmad and Rademakers, Fons and Luna, Juan G\'{o}mez and Mutlu, Onur},
  title = {High-Performance and Scalable Agent-Based Simulation with BioDynaMo},
  year = {2023},
  isbn = {9798400700156},
  publisher = {Association for Computing Machinery},
  address = {New York, NY, USA},
  url = {https://doi.org/10.1145/3572848.3577480},
  doi = {10.1145/3572848.3577480},
  booktitle = {Proceedings of the 28th ACM SIGPLAN Annual Symposium on Principles and Practice of Parallel Programming},
  pages = {174–188},
  numpages = {15},
  keywords = {NUMA, HPC, performance evaluation, parallel computing, space-filling curve, high-performance simulation, performance optimization, agent-based modeling, memory layout optimization, memory allocation, scalability},
  location = {Montreal, QC, Canada},
  series = {PPoPP '23},
  archivePrefix = "arXiv",
  eprint        = "2301.06984",
  primaryClass  = "cs.DC"
}
```

We recommend specifying the exact BioDynaMo version used in the methods section. 
For example: 

``` latex
We use BioDynaMo v1.04.5-e9db03a1 \cite{breitwieser_biodynamo_2022, breitwieser_biodynamo_2023} 
for all simulations in this article. 
```

The version can be obtained by executing `biodynamo --version` and can be interpreted as follows: `vMAJOR.MINOR.PATCH-SHA` 
`SHA` is the git commit hash that can be used to check out the exact BioDynaMo version with `git checkout SHA`.

## License

BioDynaMo is [Apache 2.0 licensed](./LICENSE).

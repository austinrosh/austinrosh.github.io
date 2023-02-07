---
title: 2.4 GHz RF Energy Harvester
date: 2021-06-14 00:00:00 -10
categories: []
tags: []
img_path: /assets/project-img/2021-06-14-rf-energy-harvester/
toc: true
comments: false
math: true
---


For my final year undergraduate project, I worked in a team on an ISM band RF energy harvesting system for powering (very) low energy loads. Despite this project happening during the peak of the COVID-19 pandemic, my partner and I were able to successfully develop and test hardware to prove this concept out in the lab! This post will give an overview and background of RF energy harvesting, describe the problem that RF energy harvesting hopes to solve, present detailed design and test results, and give a roadmap of potential future applications.

If you are interested in the full project report, it can be found on the Santa Clara University Scholar Commons [here](https://scholarcommons.scu.edu/elec_senior/62/).

![rfeh_map](rfeh_mindmap.png){: width="500" height="4000" }
_The many potential applications of RF energy harvesting._

# Overview and Motivation for RF Energy Harvesting
The idea of wireless power transfer (WPT) has existed since the rise of modern electrical engineering in the late 19th century. Nikola Tesla was the first to conceive of the usefulness of WPT, declaring the ability to transfer energy without physical connections or wiring as, “an all-surpassing importance to man” \cite{tesla_wpt}. However, a limitation of WPT systems are low-input powers, not only making system design difficult, but also restricting its application to specific types of loads.

Fortunately, WPT and radio-frequency energy harvesting (RFEH) are now seen as promising future technology due to recent advances in CMOS technology and the ability to utilize low-input powers in small form-factor devices. The exponential increase in electrical computing efficiency is known as \textit{Koomey's Law}, which states that the number of computations per Joule of energy dissipated doubles about every 1.57 years \cite{koomey_MooresLaw}. This trend is visualized in Fig. \ref{fig:koomeys_law} over the time period of 1940 to 2010.

![koomeys law](koomeys_law.png){: width="500" height="4000" :label }
_Number of computations per micro-Joules averaged over an hour. A micro-Joule is the sum of 1-$\mu$W power during 1s, or 100 nW during 10s \cite{koomey_MooresLaw}_

See figure [koomeys law]

The trend of increasing computational efficiency indicates a future where many devices will operate with minimal power consumption. As a result, RFEH systems will be able to act as practical power sources for a select class of devices. A potential target application of RFEH are Internet of Things (IoT) devices. In the coming years, IoT low-power sensors are expected to be an ubiquitous technology, and RFEH provides a potentially low-cost and enduring alternative to batteries. 

\par Comprised of large sensor networks connected via communication channels, IoT grids require complex wiring schemes or dedicated batteries at the device level \cite{wptalgorithms}. As the world becomes more and more connected, the efficient manufacturing and maintenance of IoT devices will become an urgent matter; by 2030, the number of connected devices is predicted to reach 125 billion \cite{techjury}. Powering these sensor networks via  wired power-grid or primary batteries will soon become infeasible due to cabling costs and the finite lifespan of batteries, which require maintenance and replacement. As an alternative solution, energy supplied by dedicated RF sources will allow IoT networks to be powered wirelessly. 

# Some Technical Aspects

In far-field WPT systems, an antenna and rectifier, abbreviated as \say{rectenna,}  carries out the process of converting RF input to usable direct current (DC) output. First developed by William C. Brown in the 1960's \cite{brown_1969}, rectennas are devices that can supply energy to traditional low-input power applications and theoretically support infinite device lifetime by removing the need for dedicated batteries with a finite energy supply. 

The challenge of designing an efficient WPT system is sustaining a high RF-to-DC conversion efficiency, as the system's efficiency is a function of input power levels, operation frequency, and discrete component properties. These factors change the input impedance of the rectifier network, causing the receive antenna to be mismatched with the rectifier, thus leading to loss of input power and lower conversion efficiency. Therefore, a realized system must be designed with the operating conditions known prior to the physical design. A simplified block diagram of an RFEH system is shown in Fig. \ref{fig:block_diagram}. Such a system has many components that introduce loss and lower the conversion efficiency from its maximum value of 100\%. Because the input-power levels of the system are already low, significant effort needs to be made to ensure proper impedance matching between components.

![block diagram](block_diagram.png){: width="500" height="4000" :label }
_Block diagram of a far-field wireless power transfer system]{Block diagram of a far-field WPT system \cite{keyrouz-thesis}. A matching network is utilized to ensure maximum power transfer at the specified operating conditions._

Many of these IoT sensors operate in conditions where it is infeasible, expensive, or dangerous to replace a battery or to deliver wired power. Examples include health monitoring sensors \cite{4403991,8567875}, structural support sensors \cite{1321506}, aircraft structural monitoring sensors \cite{Zhao_2007}, and sensors utilized in hazardous environments. Overall, the requirements for such sensors include:
1. Small size
2. Low maintenance
3. Unknown exact location

This project aims to meet these system requirements in a manner that maximizes system efficiency by carefully designing each component of a WPT network.

A detailed block diagram of the implemented RFEH system is depicted in Fig. \ref{fig:block_diagram}. The performance parameters which affect system operation are highlighted along with each component. A successful RFEH system attempts to optimize all of these factors simultaneously in order to keep the DC output voltage at a level required by the load application.

The system primarily consists of two elements: the transmitter and receiver. The transmitter has an associated transmit power and carrier frequency, and can either function as an ambient or dedicated source. An ambient source is one that exists for some other purpose, such as mobile broadcast stations or routers used for wireless communications. These sources may have an associated signal modulation scheme which can impact performance. In contrast, a dedicated source is specifically designed to supply power to a select receiver/load side, resulting in higher efficiencies. The transmit antenna can be optimized depending on the target application. For example, a high gain antenna is preferred in situations where the rectenna's location is known, allowing for the transmit antenna to supply power over a greater distance compared to an omnidirectional antenna.

Between the transmitter and receiver is a free-space region.  An energy harvesting system that captures energy sent over a free-space region greater than the far-field distance $R>2D^2/\lambda$ (where $D$ is the largest linear dimension of the transmit or receive antenna and $\lambda$ is the operating wavelength) is classified as a \textit{far-field} RF energy harvester. The received power levels for a far-field energy harvesting system can be accurately modeled by the Friis transmission equation \cite{Balanis-2012-antenna} 

$$
    P_{\mathrm{rx}}=P_t\left(1-\left|\Gamma_{t}\right|^{2}\right)\left(1-\left|\Gamma_{r}\right|^{2}\right)\left(\frac{\lambda}{4 \pi R}\right)^{2} G_{t}\left(\theta_{t}, \phi_{t}\right) G_{r}\left(\theta_{r}, \phi_{r}\right)\left|\hat{\rho}_{t} \cdot \hat{\rho}_{r}\right|^{2}, \tag{1}
    \label{eq:Friis} 
$$ 

where the received power $$P_\mathrm{rx}$$ is a function of the power transmitted ($$P_{t}$$), the matching level of the transmitter and receiver ($$\Gamma_{t}$$ &, $$\Gamma_{r}$$), the operating wavelength ($$\lambda$$), the transmitter-to-receiver separation distance ($$R$$), the gain of the transmit and receive antennas ($$G_t$$, $$G_r$$), and polarization mismatch between the transmit and receive antennas 
$$
\left|\hat{\rho}_{t} \cdot \hat{\rho}_{r}\right|^{2}. 
$$

On the receive side, the first element is the antenna. The antenna is designed to be low-gain such that it can receive input RF energy from any direction. The antenna operates over a specified bandwidth to capture the incident carrier signal. Additionally, the antenna must be matched to a nominal system impedance to minimize reflection losses and ensure maximum power transfer. In this case, a nominal system impedance of $Z_0 = 50\;\Omega$ is used.

Likewise, the rectifier needs to be matched to the system impedance. However, challenges arise as the rectifier's input impedance non-linearly varies as a function of frequency, input power, and load impedance. Thus, a robust simulation model that can capture and represent these effects is crucial for the design of the system.

The output of the rectifier is interfaced with a power management unit (PMU) to store energy and drive the load application. Ideally, the PMU operates in a region that presents a fixed DC resistance to the rectifier, allowing the rectifier to be optimally matched to this value. The load application has its own specifications, such as power consumption and usage period, in addition to minimum driving conditions required for the system to initially be powered.

![block9](block9.png){: width="650" height="500" :label }
_Block diagram of implemented system._


<!---
More about non-linear diode features, impedance matching, system integration, etc.
-->

# System Design

## Antenna
The receive antenna was designed to both capture incident power densities and present an input impedance close to the system impedance $$Z_0$$ at the design frequency $$f_{0}$$. Many different antenna topologies can be utilized in RFEH systems, however, with one of the primary system objectives being a low-profile geometry, a planar microstrip based antenna was selected as the antenna type. Microstrip antennas have a simple initial design process, which can then be tuned to achieve certain performance results such as polarization diversity or dual-band operation \cite{Balanis-2012-antenna}. Additionally, microstrip antennas can utilize various feed networks, allowing for various system layout configurations, and can be efficiently integrated onto a PCB sharing analog and digital circuitry.

The antenna topology chosen for the final design was the **{planar inverted F antenna} (PIFA)**. The PIFA consists of a rectangular planar element placed above a ground plane, a short-circuit pin, and a microstrip feed. The PIFA layout used in HFSS simulations is depicted in Fig. \ref{fig:PIFA_annotated}.

![pifa annotated](IFA_annotated.png){: width="650" height="500" :label }
_PIFA geometry and PCB dimensions._

The PIFA is a variant of a standard monopole antenna, where the top section has been folded to be parallel to the ground plane, effectively creating the image of a dipole. This also reduces overall antenna height while maintaining a length required for resonance. The arm denoted by length $$L_\mathrm{1}$$ forms a shunt capacitance to ground, which is inductively tuned by the short-circuit stub of length $$L_\mathrm{2}$$.

The ground plane plays a crucial role in the operation of the PIFA. Exciting currents in the PIFA causes excitation of currents in the ground plane. The radiated electromagnetic field is a result of the interaction between the PIFA and its image below the ground plane. The radiation performance is dependent on the geometry of the ground plane, and is typically enhanced when the ground plane is much greater than the dimensions of the antenna itself. In general, the width of the ground plane should be at least as wide as the PIFA $$L_\mathrm{1}$$ arm, while the length should be at least $$\lambda/4$$ in height \cite{Balanis-2012-antenna}, where $$\lambda$$ is the free-space wavelength at the operating frequency.

The dimensions of the PIFA are listed in Table \ref{tab:PIFA_dim}. The resonant frequency of the antenna is determined by the lengths of the radiating arm, shorting pin, and trace width, and the location of the feed relative to the shorting pin \cite{Balanis-2012-antenna}. The resonant frequency is specified such that $$L_\mathrm{1}+L_\mathrm{2}+\left(\mathrm{Trace\;Width}\right) = \lambda/4$$ at $$f_0$$. In this case, $$\lambda$$ is the free-space wavelength at $f_0$. These values are fine-tuned using the Optometrics tool in HFSS to get an optimal resonant behavior at $f_0$. The initial design frequency used in all the simulations is $$f_0 = 2.45$$~GHz, however this value can be adjusted by means of a matching network (i.e. $$f_0 = 2.1$$~GHz), as it is for the final system implementation discussed in later sections.

<>Table


The numerically tuned antenna is simulated in HFSS to characterize its directivity and efficiency, as measuring these parameters in a lab requires extremely specialized equipment and highly controlled test environments which were not accessible for this project. As seen in Fig. \ref{fig:PIFA_gain}, the gain of the antenna is fairly omnidirectional with a peak gain of $3.7$ dBi. This gain value is used in theoretical link-budget calculations involving the fabricated design as this parameter was not able to be characterized without an anechoic chamber. 

![IFA rad gain](ifa_rad_gain.png){: width="450" height="400" :label }
_Simulated PIFA gain pattern._

The measured PIFA is tuned to an operational frequency of $f_0 = 2.1$ GHz by means of a $\pi$-topology matching network. The lumped component values along with the reflection coefficient $S_{11}$ response of the system is depicted in Fig. \ref{fig:PIFA_match_2p1}.

![IFA rad gain](matched_ant_2p1GHz.png){: width="450" height="400" :label }


Smith Chart            |  Log Magnitude
:-------------------------:|:-------------------------:
![](matched_ant_2p1GHz_smith.png){: width="450" height="400" :label }|  ![](matched_ant_2p1GHz_s11.png){: width="450" height="40" :label }


## Rectifier

The rectifier is responsible for converting an input AC signal to a DC output that can then be used to power application-specific loads. In order to ensure maximum power transfer between the antenna and rectifier, reflection should be reduced and the rectifier should be matched to the system impedance, i.e. $Z_0 = 50~\Omega$. A voltage doubler circuit, shown in Fig. \ref{fig:voltagedoubler}, and an input power of -10 dBm were chosen based on their prevalence in RFEH literature \cite{wptalgorithms}, \cite{8314461}, \cite{5434266}, \cite{vissersensors}. Here, $\mathrm{Z_{source}}$ denotes where the matching network will be placed, which is implemented to match the fundamental frequency's input impedance to the nominal system impedance $Z_0$ for a specific input power and load resistance.

## PCB: Integrated Rectenna

# System Testing

## Rectifier Performance

## Over-the-Air Performance

# An Application

# Future Applications

# Conclusions

The goal of this project was to display the validity of RFEH by designing and validating our own custom solution capable of powering an IoT sensor. This work was motivated by an increasing reliance on IoT devices, and thus, a demand for more power to be delivered in an efficient and sustainable fashion. In doing so, robust simulations were conducted using powerful solvers and modern techniques such as ADS and HFSS. The design was then laid out and fabricated. Extensive tests in different conditions validated our system and provided proof of the system's ability to power a commercial IoT temperature sensor. Assessing our performance, original objectives were achieved and when faced with technical challenges, the project was appropriately adapted in a manner that successfully validated our system.

We have outlined a few tasks to further improve our design:
1. Performing electromagnetic co-simulation of the PCB with lumped components and diode models, thereby providing a comprehensive simulation that accounts for component and trace parasitics
2. Integrating the PMU and sensor load onto the remaining PCB area to create a compact and comprehensive system
3. Analyzing the fabricated antenna in an anechoic chamber for OTA characterization
4. Exploring input signal optimization to improve RF-to-DC conversion efficiency.

Overall, this project has demonstrated many of the exciting and emerging applications of RFEH, and has served to open innovation for future researchers.

# References

[^fn-xx]: the footnote source
[^fn-x2]: the other footnote source
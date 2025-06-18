# Introduction to the Flexcharge Simulation Platform

## Abbreviation

| Full Name | Abbreviation |
|-------|-------------|
| CP  |Charging Pile|
|#| number of|
|MP | Measurement Point |


## 1. Introduction

FlexCharge is a data-driven flexible charging simulation platform designed with a modular architecture. Aimed at the operational management of CPs, the platform enables data-driven modeling, simulation, evaluation, analysis, and decision-making to help CPs achieve intelligent charging scheduling and pricing in line with their own interests. Currently, the platform primarily offers two major functions: visualizing the value of charging scheduling and providing a platform for diversified research.

The name "FlexCharge" carries two meanings:

1. **Flexible Charging Coordination**: It aims to collaborate with power grids and operators to enable flexible charging for electric vehicles (EVs). Through optimized scheduling, it seeks to fully leverage charging flexibility to improve system efficiency.

2. **Flexible Interaction with EV Owners**: It intends to obtain charging control rights through dynamic and customized pricing services that align with owners' charging preferences. This flexible interaction aims to achieve two practical goals:
- Convincing operators of the benefits of flexible charging services;
- Addressing existing technical challenges, including online scheduling, battery health management, and mechanisms for interacting with EV owners.


### 1.1 Platform Background

Transportation electrification is a critical strategy for countries worldwide to address climate change and achieve energy transition. With the popularization of electric vehicles (EVs), their charging activities have become increasingly impactful on power grids. How to effectively manage charging to meet demand while mitigating the grid impact of massive charging loads—and even leveraging charging flexibility to assist grid operations—has become a key concern for power utilities, CP operators, and EV owners alike.

#### Perspectives and Challenges:
- **Power Utilities**:
  EV charging demand and the integration of renewable energy are highly stochastic, posing severe challenges to grid stability. However, the flexibility of charging loads can help smooth peak-valley differences, absorb renewable energy, and enable EVs (as mobile energy storage) to redistribute loads spatially.
- **CP Operators**:
  Scheduling charging in response to electricity prices, emission reduction signals, or providing auxiliary services can reduce carbon emissions, lower power costs, and generate additional revenue.
- **EV Owners**:
  They can flexibly choose charging plans and enjoy price incentives without affecting vehicle use, thereby reducing charging costs.

#### Key Hurdles in Flexible Charging Management:
1. **Uncertainty in EV Usage**:
   Owners’ travel plans may change dynamically, affecting the flexibility of pre-scheduled charging tasks and subsequent dispatch.
   - Factors like diverse battery technologies, models, specifications, and battery health (e.g., actual energy demand vs. rated capacity) further complicate charging performance prediction.

2. **Grid Integration Challenges**:
   Charging management can enhance economic benefits by adapting to grid signals (e.g., adjusting to time-of-use pricing or tracking grid-determined power signals for "ancillary services"). However, the randomness of future signals in real-time charging decisions often undermines potential gains from grid participation.

3. **Underdeveloped Digitalization**:
   Despite existing CP management platforms, their overall digital and intelligent capabilities remain limited, failing to fully exploit the flexibility and latent economic value of charging management due to multiple uncertainties.

#### Digital Twin Technology as a Solution:
The digitalization and intelligence of EV charging management rely heavily on real-time interaction between physical systems and management platforms, demanding advanced sensing, communication, and computing capabilities. **Digital twin technology**, an emerging approach, addresses these challenges by providing a virtual environment that accurately simulates physical systems.
- Unlike traditional simulation, digital twins maximize the use of historical and real-time data to extract critical insights.
- They also enable proactive evaluation and decision feedback to optimize physical system performance.

For EV charging management, digital twins offer:
- A virtual platform to enhance historical data mining, real-time process monitoring, and decision-making.
- Post-hoc evaluation of economic value by re-examining historical decisions, while leveraging historical and real-time data to unlock unknown potential in real-time scheduling.

#### The Mission of FlexCharge:
Against this backdrop, **FlexCharge aims to explore the platform-based application of digital twin technology for EV charging management**. By establishing a theoretical framework for data analysis and real-time decision-making, the platform seeks to realize the latent economic value of charging management.
- We have built a modular digital twin platform that simulates real-world EV CPs, adaptable to diverse management scenarios.
- FlexCharge aims to overcome operational bottlenecks in most existing CP platforms, maximizing benefits for power grids, operators, and EV owners.


## 2 Platform Architecture

The platform adopts an overall modular design and currently consists of five major modules: **Data Input, Physical Model, Algorithm, UI, and Simulation Calculation**, as shown in the figure below.

<figure>  
  <img src=./fig1.png>  
  <figcaption style="text-align: center;">Figure 1: Overall Structure of FlexCharge</figcaption>  
</figure>

#### 1. **Data Input Module**
Charging demand data is the most basic and essential input. Each charging task typically includes: **vehicle arrival time, departure time, required energy, and maximum charging power**. A series of charging tasks arranged by arrival time constitutes the charging demand over a period. Such data usually originates from real charging records but can also be artificially generated based on the probability distribution of historical data. In the future, it can be input in real time through closed-loop interaction with actual CPs.
- First, the input charging demand data is processed into a **charging demand queue** required for online charging scheduling simulation, enabling the simulation of EV arrivals and departures.
- In the platform’s simulation tests, charging demands are input into the system in real time (i.e., future demands are invisible), requiring the platform to use online algorithms to make real-time charging decisions.
- Additionally, depending on scenarios and task requirements, external data can be fed into the system, including: locally fluctuating wind/solar power generation, time-varying distribution network electricity prices, and demand-side response signals from power companies.


#### 2. **Physical Model Module**
This module primarily includes **multi-precision modeling of charging connection networks and batteries**:
- Factors such as CP transformer capacity, three-phase connection modes, and CP configurations all affect charging flexibility and system efficiency.
- The charging/discharging processes and capacity degradation of batteries are influenced by complex electrochemical reactions, making accurate evaluation highly challenging. Balancing the accuracy of quantitative assessment with the computational feasibility of decision-making is a key problem the platform aims to solve.


#### 3. **Algorithm Module**
This module contains online algorithms based on different principles, mainly categorized into three types: **Sorted Scheduling, Model Predictive Control, and Novel Performance-Driven Algorithms**.
- **Sorted Scheduling**: Include Round Robin scheduling, first come first served (FCFS), last come first served (LCFS), and least laxity first (LLF) (e.g., urgent tasks first).
- **Model Predictive Control**: Establishes a time-domain coupled optimization problem based on global objectives, using predicted values for future parameters. By solving this problem, it determines current charging decisions (considering future impacts) and repeats the process iteratively. This rolling optimization framework can accommodate multiple decision objectives, making it widely applicable with strong practical effectiveness.
- **Novel Performance-Driven Algorithms**: Custom-designed algorithms addressing specific technical challenges, with rigorous performance guarantees under appropriate parameter assumptions. The platform also enables testing how these algorithms perform in generalized real-world scenarios.


#### 4. **UI Module**
The UI is divided into two parts for **CP operators** and **EV owners**:
- **Operator UI**: Allows operators to set hyperparameters (e.g., CP configurations) and operational parameters (e.g., objectives, algorithms), enabling comprehensive evaluation and utilization of charging flexibility.
- **Owner UI**: Serves as a key medium for interacting with EV owners. By learning owners’ charging preferences through their choices, the platform provides customized pricing options, ensuring incentive compatibility while acquiring charging control rights.


#### 5. **Simulation Calculation Module**
This module efficiently computes **system state transitions and evaluation metrics** based on inputs from data, models, and decisions, providing real-time feedback for decision-making. Specifically, it:
- Simulates the dynamic impacts of charging scheduling on the grid, CPs, and EV owners;
- Generates multi-dimensional evaluation indicators (e.g., charging efficiency, cost, user satisfaction) to support algorithm optimization and strategy adjustment.


**The following introduces key modules and functions.**

### 2.1 Introduction to the Algorithm Module

In FlexCharge, different charging algorithms can be selected to achieve varied objectives. The algorithms currently available in FlexCharge include: **Uncontrolled Charging**, **Sorted Scheduling** (e.g, Round Robin, first come first served, earliest deadline first (EDF), least laxity first), and **Model Predictive Control** with different objectives (e.g., quick charge objective, cost minimum objective).

Researchers can easily test the performance and effectiveness of their own algorithms by replacing them in FlexCharge. They can also compare algorithm effects to determine their advantages and disadvantages.

**Algorithm Descriptions:**

- $Uncontrolled\ Charging$: This algorithm ignores all infrastructure constraints. All EVs will be charged as quickly as possible according to their maximum charging rate and their battery dynamics.
- $Sorted\ Scheduling$: Active EVs are first sorted by some metric, then current is allocated to each EV in order. To allocate current we use a binary search approach which allocates each EV the maximum current possible subject to the constraints and already allocated allotments:
  - $First\ come\ first\ served$: Active EVs are first sorted by their arrival time.
  - $Least\ laxity\ first$: Active EVs are first sorted by their $laxity$, which is defined by $laxity=d_i-\frac{e_i}{\overline{r}_i}$, where $d_i$ is the estimated departure time, $\overline{r}_i$ denotes the maximum charging rate, and $e_i$ is the requested energy. 
  - $Last\ come\ first\ served$: Active EVs are first sorted by their arrival time in an reversed manner.
  - $Earliest\ Deadline\ First$: Active EVs are first sorted by their estimated departure time.
  - $Largest\ remaining\ processing\ time$: Active EVs are first sorted by their remaining processing time $\Delta_i$, which is defined by $\Delta_i=\frac{e_i}{\overline{r}_i}$.
- $Round\ Robin$: Schedule EVs using a round-robin-based equal sharing scheme. Specifically, each EV is allocated a charging rate in a cyclic order, starting from the minimum feasible rate. The allocation continues until the charging rate exceeds the feasible range defined by the EV’s constraints and infrastructure limits.
- $Model\ predictive\ control$: Given an objective function, this algorithm updates charging schedules whenever a new time step occurs or a new event is detected. It does so by re-optimizing the objective function based on the latest demand and system conditions.
  - $quick\ charge\ obj$: Suppose the optimization horizon is $T$, and the charging rate at each time step is $r_t$, then the objective function is given by $\sum_{t=1}^T\frac{T-t}{T}r_t-10^{-12}\cdot\sqrt{\sum_{i=1}^Tr_t^2}$. The first term is to encourage fast charging, and the second term is to encourage equal distribution of charging rates.
  - $cost\ min\ obj$：This objective is used to minimize the cost for providing the charging service. Suppose the optimization horizon is $T$, and the charging rate at each time step is $r_t$, the total energy deliverved at time step $t$ is $e_t$, the electricity price is $p_t$, the demand charge unit price is $p_d$, and the peak power is $P_{max}$. The objective function is given by: $$\sum_{t=1}^T(1000-p_t)e_t-p_dP_{max}+10^{-4}\cdot\sum_{t=1}^T\frac{T-t}{T}r_t-10^{-12}\cdot\sqrt{\sum_{i=1}^Tr_t^2}$$
- $Offline$: This algorithm performs offline optimization for a given objective function, assuming perfect future information. The optimization process is based on idealized models of EVSEs and batteries. If non-ideal models are used, the results may not be optimal or feasible.
  - $quick\ charge\ obj$: same as above
  - $cost\ min\ obj$: same as above

### 2.2 Result Visualization

The metrics currently mainly include system parameter information, electrical indicators (total energy delivered after charging scheduling, ratio of completed sessions, maximal current and power of the CP, electricity cost expenditure of the CP, demand charges generated by power supply, fulfillment ratio of the number of electric vehicle chargings, total delivered mileage), and estimated carbon indicators (estimated carbon emission reduction, estimated number of trees for carbon sequestration). Through these overall metrics, CP operators can reflect on the key factors leading to operational inefficiencies in combination with the benefits generated by actual decisions. In the long run, combined with the upfront investment in the CP, the marginal benefits of various resource allocations can be analyzed to provide guiding suggestions for subsequent planning. The diagram showing the metric results is as follows:

<figure>
  <img src=./fig2.png>
  <figcaption style="text-align: center;">Figure 2: Metric Results</figcaption>
</figure>



The chart visualization is currently divided into data visualization and result visualization after scheduling operations. Through the visualization of charging demand data, users' charging behavior habits can be well characterized, and the charging flexibility when a large number of users charge can be displayed. The visualization of charging demand data is shown in the following figure, mainly including the time of user arrival and departure, the charging demand of electric vehicles, the flexibility of electric vehicles, charging demand - charging duration, and the distribution of arrival time - charging demand - charging duration. As shown in the figure below, by visualizing user behavior, users' charging patterns can be well grasped, thereby better arranging charging.

<div style="text-align: center;">
  <figure>
    <img src=./fig3_1.png>
  </figure>
  <figure>
    <img src=./fig3_2.png>
  </figure>
  <figure>
    <img src=./fig3_3.png>
  </figure>
</div>
<figcaption style="text-align: center;">Figure 3: Visualization of Charging Demand Data</figcaption>

Another type of chart visualization is the visualization of flexible charging scheduling results. We have refined the scheduling results both temporally and spatially. This mainly includes the aggregate current changes, power changes, utilization of CPs, cumulative delivered energy, power, and current of an individual CP. Through temporal and spatial refinement, the operational effects of the charging scheduling decision algorithm running in a specific actual CP can be intuitively seen. At the same time, it can be judged whether the large-scale charging process violates the physical constraints of the CP's own infrastructure and whether the expected charging goals are achieved when flexible charging is used or not. Based on the above results, CP operators can make more informed, data-driven decisions. (The visualization of the value of flexible charging should not be limited to these; further exploration is needed, both for metric-based results and chart-based results.) The demonstration diagrams are as follows:

<div style="text-align: center;">
  <figure>
    <img src=./fig4_1.png>
  </figure>
  <figure>
    <img src=./fig4_2.png>
</figure>
</div>
<figcaption style="text-align: center;">Figure 4: Visualization of Flexible Charging Results</figcaption>

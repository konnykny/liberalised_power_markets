# liberalised_power_markets

| Symbol                                       | Dimensions         | Type         | Unit        | Description                                                                           |
| -------------------------------------------- | ------------------ | ------------ | ----------- | ------------------------------------------------------------------------------------- |
| **SETS**                                     |                    |              |             |                                                                                       |
| N                                            | {1,2,3,4,5}        | Set          | –           | Nodes in the network                                                                  |
| H                                            | {1,…,24}           | Set          | –           | Hours in a representative day                                                         |
| S                                            | {1,2,3,4}          | Set          | –           | Seasons (1 = Winter, 2 = Spring, 3 = Summer, 4 = Autumn)                              |
| T                                            | {1,2,3,4,5}        | Set          | –           | Generation technologies (1 = Wind, 2 = Nuclear, 3 = Coal, 4 = Gas OCGT, 5 = Gas CCGT) |
| L                                            | 10 tuples          | Set          | –           | Transmission lines (all combinations of 2 nodes from 5)                               |
| SC                                           | {1,2,3}            | Set          | –           | *Stochastic only*: Scenarios (1 = Pessimistic, 2 = Expected, 3 = Optimistic)          |
| **DECISION VARIABLES (Deterministic Model)** |                    |              |             |                                                                                       |
| p                                            | T × H × N × S      | Variable     | MWh         | Generation level per technology, hour, node, season                                   |
| p_max                                        | T × N              | Variable     | MW          | Additional (new) generation capacity by technology and node                           |
| q                                            | H × N × S          | Variable     | MWh         | Energy shedding (load not served) per hour, node, season                              |
| f                                            | H × L × S          | Variable     | MWh         | Transmission flow per hour, line, season (unconstrained)                              |
| f_abs                                        | H × L × S          | Variable     | MWh         | Absolute value of transmission flow (auxiliary variable)                              |
| f_max                                        | L                  | Variable     | MW          | Additional (new) transmission capacity per line                                       |
| **DECISION VARIABLES (Stochastic Model)**    |                    |              |             |                                                                                       |
| p                                            | T × H × N × S × SC | Variable     | MWh         | Generation level (stochastic variant)                                                 |
| q                                            | H × N × S × SC     | Variable     | MWh         | Energy shedding (stochastic variant)                                                  |
| f                                            | H × L × S × SC     | Variable     | MWh         | Transmission flow (stochastic variant)                                                |
| f_abs                                        | H × L × S × SC     | Variable     | MWh         | Absolute transmission flow (stochastic variant)                                       |
| p_max                                        | T × N              | Variable     | MW          | Additional generation capacity (here-and-now decision)                                |
| f_max                                        | L                  | Variable     | MW          | Additional transmission capacity (here-and-now decision)                              |
| **DUAL VARIABLES**                           |                    |              |             |                                                                                       |
| λ                                            | H × N × S          | Dual         | €/MWh       | Shadow price / nodal price (dual of energy balance constraint)                        |
| **GENERATION PARAMETERS**                    |                    |              |             |                                                                                       |
| KI                                           | T = 5              | Scalar Array | k€/MW       | Generation investment cost per technology                                             |
| LT                                           | T = 5              | Scalar Array | years       | Generator lifetime per technology                                                     |
| K                                            | T = 5              | Scalar Array | k€/MW       | Annualized generation investment cost                                                 |
| KA                                           | T = 5              | Scalar Array | k€/MW       | Annual generation maintenance cost                                                    |
| CO                                           | T = 5              | Scalar Array | k€/MWh      | Generation operational (fuel) cost per technology                                     |
| GCN                                          | T × N = 5×5        | Array        | MW          | Existing (initial) generation capacity                                                |
| max_load                                     | N = 5              | Array        | MW          | Maximum load per node (used for normalization)                                        |
| **DEMAND & WIND PARAMETERS**                 |                    |              |             |                                                                                       |
| D                                            | N × H × S          | Array        | MWh         | Demand per node, hour, season (deterministic)                                         |
| W                                            | N × H × S          | Array        | [0,1]       | Wind availability factor per node, hour, season                                       |
| D_sto                                        | N × H × S × SC     | Array        | MWh         | Demand (stochastic variant: min, mean, max)                                           |
| W_sto                                        | N × H × S × SC     | Array        | [0,1]       | Wind availability (stochastic variant)                                                |
| **TRANSMISSION PARAMETERS**                  |                    |              |             |                                                                                       |
| KIT                                          | Scalar             | Scalar       | k€/(MW·km)  | Investment cost per unit capacity per km                                              |
| dist                                         | L = 10             | Array        | km          | Length of each transmission line                                                      |
| KT_per_line                                  | L = 10             | Array        | k€/MW       | Annualized transmission investment cost per line                                      |
| KAT                                          | Scalar             | Scalar       | k€/MW       | Annual transmission maintenance cost                                                  |
| COT                                          | Scalar             | Scalar       | k€/MWh      | Transmission operational cost                                                         |
| LT_T                                         | Scalar             | Scalar       | years       | Transmission line lifetime                                                            |
| **CONSTRAINT & POLICY PARAMETERS**           |                    |              |             |                                                                                       |
| CS                                           | Scalar             | Scalar       | k€/MWh      | Penalty cost for shedding (1.0)                                                       |
| ShC                                          | Scalar             | Scalar       | %           | Shedding limit as percentage of demand (0.1 = 10%)                                    |
| RES                                          | Scalar             | Scalar       | %           | Minimum renewable energy share system-wide (0.25 = 25%)                               |
| RES_node                                     | N = 5              | Array        | %           | Minimum renewable energy share per node (Task 2)                                      |
| ramp_limit                                   | Scalar             | Scalar       | %           | Ramping constraint relative to max capacity (Task 2)                                  |
| SS / season_days                             | S = 4              | Array        | days        | Number of days per season [92, 91, 91, 91]                                            |
| Pr                                           | SC = 3             | Array        | probability | Probability of each scenario (stochastic)                                             |
| **HELPER STRUCTURES**                        |                    |              |             |                                                                                       |
| line_index                                   | Dictionary         | –            | –           | Maps line tuple (i,j) to index 0–9                                                    |
| inc(n,l)                                     | Function           | –            | {−1,0,1}    | Network incidence: flow direction sign per node–line pair                             |



## Notes

- Five generation technologies: 
    - Wind
    - Nuclear
    - Coal
    - CCGT
    - OCGT
- Renewables = wind >= 25 %
- Transimission line limitations
- 24h nodal Demand profiles (hourly timeline) 
- Seasons represented by one day demand profile each
-  wind profiles are Season- and node-dependent

### Pulp optimization docu:
https://coin-or.github.io/pulp/

| Concept     | Gurobi (`gurobipy`) | PuLP            |
| ----------- | ------------------- | --------------- |
| Model       | `Model()`           | `LpProblem()`   |
| Variables   | `addVar()`          | `LpVariable()`  |
| Objective   | `setObjective()`    | `+=` expression |
| Constraints | `addConstr()`       | `+=` expression |
| Sense       | `GRB.MINIMIZE`      | `LpMinimize`    |
| Solve       | `model.optimize()`  | `model.solve()` |
| Value       | `var.X`             | `value(var)`    |


## Tasks

network:
10 lines —> fehlende ergänzen
Preisverlauf platten



Nächstes treffen: nächsten Freitag, 6.2., 12:30
Jan:
- Nr 3 validieren

Konny:
- Fehlende Linien ergänzen 
- Plots export zu overleaf
- Aufgabe 2 u 3 drüberschauen, vergleichplots 
- Prozentuelle veränderung (Erzeugung und grid) und Strompreis

Philipp
- Einleitung 
- Kapitel 1 modell definieren
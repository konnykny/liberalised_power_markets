# liberalised_power_markets




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

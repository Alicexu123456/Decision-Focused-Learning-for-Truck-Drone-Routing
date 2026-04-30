# Decision-Focused-Learning-for-Truck-Drone-Routing
We've developed a black-box solver to potentially solve all truck-drone routing problem variants with various objectives, which mainly utilizes LKH-3. Then, to consider the uncertain truck/drone travel times, we want to make best decisions before true uncertain parameters are realized. Hence, this project applies Decision-Focused Learning (DFL) methods to solve the simplest truck-drone routing (Flying Sidekick TSP, FSTSP with one truck and one drone). 

Oracle method: current solver

Baseline method (PtO): Decision-blind Predict-then-Optimize makes the best predicion on travel times, then call the solver

Six DFL methods: 
(1) BlackBox Backprop (DBB). Vlastelica, M., Paulus, A., Musil, V., Martius, G., & Rolínek, M. (2019). Differentiation of blackbox combinatorial solvers.
(2) Randomized Smoothing (RS). Dalle, G., Baty, L., Bouvier, L., & Parmentier, A. (2022). Learning with combinatorial optimization layers: a probabilistic approach.
(3) Perturbation Gradient Backward loss (PGB). Gupta, V., & Huang, M. (2024). Decision-focused learning with directional gradients.
(4) Perturbation Gradient Center loss (PGC). Gupta, V., & Huang, M. (2024). Decision-focused learning with directional gradients.
(5) Smart Predict-then-Optimize (SPO+). Elmachtoub, A. N., & Grigas, P. (2022). Smart “predict, then optimize”.
(6) Perturbed Fenchel-Young Loss (PFYL). Berthet, Q., Blondel, M., Teboul, O., Cuturi, M., Vert, J. P., & Bach, F. (2020). Learning with differentiable pertubed optimizers.





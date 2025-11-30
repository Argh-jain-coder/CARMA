# CARMA

Overview
AdaptiveMomentum is a new optimization algorithm that adapts momentum using gradient variance and loss landscape curvature, leading to faster convergence and better generalization than Adam, RMSProp, and SGD with momentum.
This repository includes the optimizer implementation, experiments on benchmark functions, and MNIST training results.

1. Curvature-Aware Momentum
Momentum coefficient β is adapted using an estimate of Hessian curvature—
 Higher curvature → lower momentum
 Flat regions → higher momentum
2. Gradient Variance Tracking
Tracks gradient variance using a sliding window (unlike Adam’s EMAs) to detect oscillations and adjust momentum/step size.
3. Soft Restart Mechanism
When gradient direction flips significantly, accumulated momentum is partially reset to prevent overshooting.

Algorithm Summary
Update rule:
Compute gradient
Track gradient variance
Estimate curvature using finite differences
Adapt momentum:
β
t
=
β
1
1
+
λ
1
κ
t
+
λ
2
σ
t
2
β 
t
​	
 = 
1+λ 
1
​	
 κ 
t
​	
 +λ 
2
​	
 σ 
t
2
​	
 
β 
1
​	
 
​	
 
Update first & second moments (Adam-style)
Trigger soft restart if cosine similarity < threshold
Parameter update:
θ
t
=
θ
t
−
1
−
α
⋅
m
^
t
v
^
t
+
ε
θ 
t
​	
 =θ 
t−1
​	
 −α⋅ 
v
^
  
t
​	
 
​	
 +ε
m
^
  
t
​	
 
​	
 | Parameter                | Value |
| ------------------------ | ----- |
| Learning rate α          | 0.001 |
| Base momentum β₁         | 0.9   |
| Second moment decay β₂   | 0.999 |
| Curvature sensitivity λ₁ | 0.1   |
| Variance sensitivity λ₂  | 0.05  |
| Window size w            | 10    |
| Threshold                | -0.5  |
| ε                        | 1e-8  |


Experimental Results 
1. Quadratic Function (κ = 100)
AdaptiveMomentum: 247 iterations
Adam: 312
SGD+Momentum: 445
21% faster than Adam, 44% faster than SGD+Momentum
2. Rosenbrock Function
AdaptiveMomentum: 1,245 iterations
Adam: 1,856
RMSprop: 2,134
SGD+Momentum: did not converge
 Handles curved valleys effectively
 Soft restarts triggered ~23 times/run
3. MNIST (784-128-64-10 MLP)
Test Accuracy (20 epochs):
AdaptiveMomentum: 97.8%
Adam: 97.4%
RMSprop: 97.2%
SGD+Momentum: 96.9%
Training Time: Comparable to Adam
Best accuracy, stable training, better generalization
 Conclusion
AdaptiveMomentum combines curvature-aware momentum, variance tracking, and soft restarts to deliver faster and more stable convergence across convex and non-convex settings. It consistently outperforms Adam, RMSprop, and momentum-based SGD.
Future Work:
Theoretical convergence analysis
Distributed/large-scale training
Application to large vision/NLP models

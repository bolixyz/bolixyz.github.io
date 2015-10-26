---
layout: post
title: Comparisons of RNN Architectures
---

In the paper *[An Empirical Exploration of Recurrent Network Architectures](http://jmlr.org/proceedings/papers/v37/jozefowicz15.pdf)*, the authors did a thorough exploration of different variations of the current popular RNN models: LSTM and Gated Recurrent Unit (GRU)

The LSTM structure:

<p align="center"><img src="http://gdurl.com/MlNW" height="200"/></p>

with the formulations:

$$
i_t = \tanh(W_{xi} x_t + W_{hi} h_{t-1} + b_i)  \\
j_t = \text{sigm}(W_{xj} x_t + W_{hj} h_{t-1} + b_j) \\
f_t = \text{sigm}(W_{xf} x_t + W_{hf} h_{t-1} + b_f) \\
o_t = \tanh(W_{xo} x_t + W_{ho} h_{t-1} + b_o) \\
c_t = c_{t-1} \odot f_t + i_t \odot j_t \\
h_t = \tanh(c_t) \odot o_t
$$

The GRU structure:

<p align="center"><img src="http://gdurl.com/GoG8" height="200"/></p>

with the formulations:

$$
r_t = \text{sigm}(W_{xr} x_t + W_{hr} h_{t-1} + b_r) \\
z_t = \text{sigm}(W_{xz} x_t + W_{hz} h_{t-1} + b_z) \\
\tilde{h}_t = \tanh(W_{xh} x_t + W_{hh} (r_t \odot h_{t-1}) + b_h) \\
h_t = z_t \odot h_{t-1} + (1 - z_t) \odot \tilde{h}_t
$$

### Findings

> The LSTM's main idea is that, instead of computing $$S_t$$ from $$S_{t-1}$$ directly with a matrix vector product followed by a nonlinearity (which causes the vanishing gradient problem), the LSTM directly computes $$\Delta S_t$$, which is then added to $$S_{t-1}$$ to obtain $$S_t$$. 
> ... just like a tanh-based network has better-behaved gradients than a sigmoid-based network, the gradients of an RNN that computes $$\Delta S_t$$ are nicer as well, since they can not vanish.

And from experimenting with different variations / modifications of the above two models, GRU outperformed LSTM on most tasks; while if ***adding a bias of 1 to the LSTM's forget gate closes the gap between them***.

The reason is that:

> But this initialization (small random weights) effectively sets the forget gate to 0.5. This introduces a vanishing gradient with a factor of 0.5 per timestep, which can cause problems whenever the long term dependencies are particularly severe. This problem is addressed by simply initializing the forget gates $$b_f$$ to a large value such as 1 or 2. By doing so, the forget gate will be initialized to a value that is close to 1, enabling gradient flow.



### Some insights on LSTM gates:

* The **forget gate** is of the greatest importance;
* The 2nd most important gate is the **input gate**;
* The **output gate** was the least important one. When removed, $$h_t$$ simply becomes $$\tanh(c_t)$$ which was sufficient for retaining most of the LSTM's performance.

However, when dropout is used, the above patterns do not hold. In one of their experiments, the LSTMs without input or output gates actually performed the best.

### References

1. *Jozefowicz, Rafal, Wojciech Zaremba, and Ilya Sutskever*. "[An Empirical Exploration of Recurrent Network Architectures](http://jmlr.org/proceedings/papers/v37/jozefowicz15.pdf)." In Proceedings of the 32nd International Conference on Machine Learning (ICML-15), pp. 2342-2350. 2015.

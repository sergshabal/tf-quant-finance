# Diffusion Models.

This module is a toolbox for generating and calibraring Diffusion
Models. A model is derived from the ```ItoProcess``` class. The model is
described in terms of the underying stochastic differential equation (SDE):

$$dX_i = a_i(t, X) dt + \sum_{i, j = 1}^n S_{ij}(t, X) dW_j,$$

where $$n$$ is the dimensionality of the process $$a_i$$ and $$S_{ij}$$ are the
drift and volatility terms of the SDE. $$\{ W_i \}_{i=1}^n$$ is an
$$n$$-dimensional Wiener process.

For example, the Geometric Brownian Motion is an underlying of the Black-Scholes
model.

A full description of the model class should contain the following methods:

  *   ```dim``` - dimensionality of the underlying SDE;
  *   ```dtype``` - dtype of the model coefficients;
  *   ```name``` - name of the model class;
  *   ```drift_fn``` - returns a callable, which is the drift function of
  the underlying SDE (corresponds to the $$a_i$$ above);
  *   ```volatility_fn``` - returns a callable, which is the volatility function
  of the underlying SDE (corresponds to the $$S_{ij}$$ above);
  *   ```total_drift_fn``` - returns a callable which is an integrated value
  of the drift function between two times, i.e.,
  $$\int_{t_1}^{t_2} a_i(t, x) dt$$;
  *   ```total_covariance_fn``` - returns a callable which is an integrated
  value of the covariance function between two times, i.e.,
  $$\int_{t_1}^{t_2} S S^{\mathrm{T}} (t, x) dt$$;
  *   ```sample_paths``` - return sample paths of the process at specified time
  points. The base class provides Euler scheme sampling if the drift and
  volatility functions are defined;
  * ```fd_method``` - returns a finite difference method for solving
  the [Kolmogorov equations](https://en.wikipedia.org/wiki/Kolmogorov_backward_equations_(diffusion))
  associated with the SDE. The corresponding
  Kolmogorov backward equation is:

  $$\frac{\partial}{\partial t} V(t, x) +  \sum_{i=1}^n a_i(t, X) \frac{\partial}{\partial x_i} V(t, x) + \sum_{i, j, k = 1}^n S_{ik} S_{jk} \frac{\partial^2}{\partial x_i \partial x_j} V(t, x) = 0.$$

  with the final value condition $$V(T, x) = u(x)$$.

  The corresponding Kolmogorov forward/Fokker Plank equation  is

  $$\frac{\partial}{\partial t} V(t, x) +  \sum_{i=1}^n \frac{\partial}{\partial x_i}  a_i(t, X) V(t, x) + \sum_{i, j, k = 1}^n \frac{\partial^2}{\partial x_i \partial x_j} S_{ik} S_{jk} V(t, x) = 0.$$

  with the initial value condition $$V(0, x) = u(x)$$.

  A minimum description of the model should only include ```dim```, ```dtype```,
  and ```name```. In order to use the provided ```sample_paths``` method,
  both ```drift_fn``` and ```volatility_fn``` should be defined.


TODO: provide description of model calibration procedure.
TODO: provide description of Ito process algebra API.

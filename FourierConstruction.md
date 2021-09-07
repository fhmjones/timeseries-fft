---
jupytext:
  formats: ipynb,md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.4
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

## Fourier Construction interactive demonstration

This is a prototype of an interactive app to demonstrate the construction of square, sawtooth, and other periodic signals using N components of the fourier series. Adapted by Jamie Bayer, based on [code described by Dr. Shyamal Bhar](https://vcfw.org/pdf/Department/Physics/Fourier_series_python_code.pdf), Department of Physics, Vidyasagar College for Women, Kolkata.

Please send any ideas for improvement to F. Jones.

```{code-cell} ipython3
from ipywidgets import interact
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import square, sawtooth, triang
from scipy.integrate import simps


def fourier_series(x, y, L, n):
    # Calculation of Co-efficients
    a0 = 2.0/L*simps(y, x)
    an = lambda n:2.0/L*simps(y*np.cos(2.0*np.pi*n*x/L), x)
    bn = lambda n:2.0/L*simps(y*np.sin(2.0*np.pi*n*x/L), x)
    
    # Sum of the series
    s = a0/2.0 + sum([an(k)*np.cos(2.*np.pi*k*x/L)+bn(k)*np.sin(2.*np.pi*k*x/L) for k in range(1,n+1)])
    return s

    
    
def plot_periodic_function(Function):
    if Function == 'Square':
        L = 1 # Periodicity of the periodic function f(x)
        freq = 1 # No of waves in time period L
        dutycycle = 0.5
        samples = 1000
        
        # Generation of square wave
        x = np.linspace(0, L, samples, endpoint=False)
        y = square(2.0*np.pi*x*freq/L, duty=dutycycle)
        
    elif Function == 'Sawtooth':
        L = 1 # Periodicity of the periodic function f(x)
        freq = 2 # No of waves in time period L
        width_range = 1
        samples = 1000

        # Generation of Sawtooth function 
        x = np.linspace(0, L, samples,endpoint=False)
        y = sawtooth(2.0*np.pi*x*freq/L, width=width_range)
        
    elif Function == 'Triangular':
        L = 1 #Periodicity of the periodic function f(x)
        samples = 501
        
        # Generation of Triangular wave
        x = np.linspace(0,L,samples,endpoint=False)
        y = triang(samples)
    
    @interact(n=(1, 50))
    def plot_functions(n):
        # Plotting
        plt.plot(x, fourier_series(x, y, L, n))
        plt.plot(x, y)
        #plt.xlabel("$x$")
        #plt.ylabel("$y=f(x)$")
        plt.title(Function + " signal reconstruction by Fourier series")


interact(plot_periodic_function, Function=['Square','Sawtooth','Triangular']);
```

```{code-cell} ipython3

```

# s-systems
#This is a amateur proyect of how to use S-timator in Power law formalism based ode (See https://webpages.ciencias.ulisboa.pt/~aeferreira/stimator/)
# In this model the exponents g11 and g12 has the property of g11 + g12 = 1, analogously g21 + g22 = 1
# stimulus is an array to modify the exponents to see their behavior.
import numpy as np
import stimator as st

m = st.read_model("""
title Bone Remodeling

-> stimulus = 1.0 * step(t, 20.0)

x1' =  a1 * (x1**(g11*stimulus))*(x2**((1 - g11)*stimulus)) - b1 * x1
x2' =  a2 * (x1**(1-g22)*stimulus)*(x2**(g22*stimulus))     - b2 * x2

g22 = 0.1
g11 = 0.1

a1  = 1
a2  = 0.004
b1  = 0.2
b2  = 0.02
init: x1 = 10,  x2 = 100
""")

sol = m.solve(tf=200)
sol.plot()


#g11stim = (-0.01, -0.1, -0.2, -0.25, 0.28, 0.29, 0.3, 0.35, 0.4) #, 0.45, 0.5, 0.55 ,0.6, 0.75, 0.8, 0.85 ,0.9, 0.95, 1.0)
#g22stim = (-0.01, -0.1, 0.2, 0.25, 0.28, 0.29, 0.3, 0.35, 0.4, 0.45, 0.5, 0.55 ,0.6, 0.75, 0.8, 0.85, 0.9, 0.95, 1.0)

g11stim = np.linspace(-0.001, 0.001, 10)

s = m.scan({'g11': g11stim}, tf=200, npoints=1000)

titles = ['$g_{11}$ = %.2f' % g11 for g11 in g11stim]
suptitlegend="s-systems"

s.plot(yrange=(0, 600), fig_size=(16, 11), titles=titles, suptitlegend=suptitlegend, show=True)

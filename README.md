# Optical Amplifier (EDFA) Performance Analysis

## Aim: 
To study the gain and noise performance of an Erbium Doped Fiber Amplifier (EDFA) using Python simulation.

## Apparatus/Software Required:
	Python (NumPy) in Google Colab

## Theory:
	EDFA amplifies optical signals in the C band (1530–1565 nm) using stimulated emission.
	Gain depends on wavelength and pump power.
	Noise figure quantifies SNR degradation.

## Formulas:
## Key Equations
## 1. Gain
Amplifier gain:
G=P_out/P_in 

Gain in dB:
G_dB=10〖log⁡〗_10 (P_out/P_in )

________________________________________
## 2. ASE Power
P_ASE=2n_sp (G-1)hνB_o

where:
	n_sp= spontaneous emission factor 
	G= amplifier gain (linear) 
	h= Planck's constant 
	ν= optical frequency 
	B_o= optical bandwidth 
________________________________________
## 3. Noise Figure
Noise Factor:
F=(SNR_in)/(SNR_out )

For EDFA:
F=2n_sp  (G-1)/G

Noise Figure in dB:
NF=10〖log⁡〗_10 (F)

For high-gain EDFAs (G≫1):
NF≈10〖log⁡〗_10 (2n_sp)

Typical commercial EDFAs have:
NF≈4"-" 6" dB"

________________________________________
## Typical Practical Values
Parameter	Typical Value
Pump wavelength	980 nm / 1480 nm
Signal wavelength	1530–1565 nm
Peak gain	25–40 dB
Saturation output power	10–23 dBm
Noise figure	4–6 dB
Fiber length	5–20 m

## Sample Output
   	Graph: Gain spectrum curve peaking near 1550 nm.
<img width="528" height="388" alt="image" src="https://github.com/user-attachments/assets/15e6aaff-d3f3-42d1-aa76-66bb575b6c93" />
<img width="518" height="469" alt="image" src="https://github.com/user-attachments/assets/cce70c57-34f5-4634-a90e-df47c4e45b62" />
<img width="561" height="461" alt="image" src="https://github.com/user-attachments/assets/0f298da2-0dc1-43fd-bab6-0641a5210748" />

## Python Code
```
import numpy as np
import matplotlib.pyplot as plt

# 1. Constants
h = 6.626e-34
c = 3e8
signal_wavelength = 1550e-9
frequency = c / signal_wavelength

# 2. EDFA parameters
n_sp = 1.2
bandwidth = 50e9
input_power = 1e-6  # 1 microwatt

# 3. Gain versus pump power
pump = np.linspace(0.01, 0.20, 50)

# Simplified EDFA gain model
gain = 1 + 45 * pump / (pump + 0.04)
gain_db = 10 * np.log10(gain)

# Calculate output power
output_power = gain * input_power

# 4. EDFA gain spectrum
wavelength = np.linspace(1530, 1565, 100)

# Simplified Gaussian gain profile
spectrum_gain = 30 * np.exp(
    -0.5 * ((wavelength - 1545) / 9)**2
)

# 5. Noise figure
noise_factor = 1 / gain + 2 * n_sp * (1 - 1 / gain)
noise_figure = 10 * np.log10(noise_factor)

# 6. ASE noise power
ase_power = 2 * n_sp * (gain - 1) * h * frequency * bandwidth

# 7. Plot 1: Gain versus pump power
plt.figure(figsize=(7, 4))
plt.plot(pump, gain_db, linewidth=2)
plt.xlabel("Pump Power (W)")
plt.ylabel("Gain (dB)")
plt.title("EDFA Gain vs Pump Power")
plt.grid(True)
plt.show()

# 8. Plot 2: Gain spectrum
plt.figure(figsize=(7, 4))
plt.plot(wavelength, spectrum_gain, linewidth=2)
plt.xlabel("Signal Wavelength (nm)")
plt.ylabel("Gain (dB)")
plt.title("EDFA Gain Spectrum")
plt.grid(True)
plt.show()

# 9. Plot 3: Noise figure versus gain
plt.figure(figsize=(7, 4))
plt.plot(gain_db, noise_figure, linewidth=2)
plt.xlabel("Gain (dB)")
plt.ylabel("Noise Figure (dB)")
plt.title("Noise Figure vs Gain")
plt.grid(True)
plt.show()

# 10. Display calculated values
print("EDFA Performance Analysis")
print("-------------------------")
print(f"Input Power       : {input_power:.2e} W")
print(f"Output Power      : {output_power[-1]:.2e} W")
print(f"Maximum Gain      : {gain_db[-1]:.2f} dB")
print(f"Noise Figure      : {noise_figure[-1]:.2f} dB")
print(f"ASE Noise Power   : {ase_power[-1]:.2e} W")
```
## Output
<img width="609" height="393" alt="image" src="https://github.com/user-attachments/assets/e09de393-a25d-4bf0-8f5c-1e690f0ac66d" />

<img width="609" height="393" alt="image" src="https://github.com/user-attachments/assets/f0be45cc-ccff-4698-9486-5180d956ecd4" />

<img width="631" height="393" alt="image" src="https://github.com/user-attachments/assets/c81e9d1c-e4b0-4337-833e-0fd2c78ce843" />


## Result
EDFA provides ~20–24 dB gain with ~5 dB noise figure.


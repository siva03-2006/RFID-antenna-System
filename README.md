# RFID-antenna-System
The system design of an RFID antenna involves understanding the frequency of operation, the environment in which the system is deployed, and the type of RFID system being used (active or passive).
import numpy as np
import matplotlib.pyplot as plt

# Constants
c = 3e8  # Speed of light in meters per second (m/s)
frequency = 900e6  # Frequency of operation (900 MHz for UHF RFID)
wavelength = c / frequency  # Wavelength (m)
P_t = 1  # Transmitted power in watts (assumed)

# Free Space Path Loss (FSPL) formula
def fspl(distance, frequency):
    """
    Calculate the free-space path loss (FSPL) in dB.
    FSPL(dB) = 20*log10(d) + 20*log10(frequency) - 147.55
    where d is the distance in meters, and frequency in Hz.
    """
    return 20 * np.log10(distance) + 20 * np.log10(frequency) - 147.55

# Signal power received (in dB) based on distance
def received_signal_power(P_t, distance, frequency):
    """
    Calculate received signal power (in dBm) using FSPL.
    """
    fspl_value = fspl(distance, frequency)
    P_r = 10 * np.log10(P_t) - fspl_value  # Convert transmitted power to dBm
    return P_r

# RFID tag detection threshold (in dBm)
detection_threshold = -70  # Typically between -60 dBm and -80 dBm

# Simulate signal propagation from 1 meter to 100 meters
distances = np.linspace(1, 100, 100)
received_powers = [received_signal_power(P_t, d, frequency) for d in distances]

# Determine at which distance the tag will be detected
detection_distance = distances[np.where(np.array(received_powers) >= detection_threshold)[0][0]]

# Plotting the received signal strength over distance
plt.figure(figsize=(10, 6))
plt.plot(distances, received_powers, label="Received Signal Power (dBm)", color='b')
plt.axhline(y=detection_threshold, color='r', linestyle='--', label="Detection Threshold")
plt.title("RFID Signal Propagation and Tag Detection")
plt.xlabel("Distance (m)")
plt.ylabel("Received Signal Power (dBm)")
plt.legend()
plt.grid(True)
plt.show()

# Output the detection distance
print(f"RFID tag detected at a distance of: {detection_distance:.2f} meters")

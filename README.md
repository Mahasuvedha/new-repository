# new-repository
import rasterio
import numpy as np
import matplotlib.pyplot as plt

def extract_lst(band10_path, band11_path):
    # Open the Thermal Infrared (TIR) bands
    with rasterio.open(band10_path) as band10, rasterio.open(band11_path) as band11:
        # Read the data as numpy arrays
        band10_data = band10.read(1, masked=True)
        band11_data = band11.read(1, masked=True)

        # Convert digital number to radiance
        radiance = (band11_data * 0.0003342) + 0.1

        # Convert radiance to temperature (in Kelvin)
        kelvin = (1260.56 / np.log(774.89 / radiance + 1)) - 273.15

        # Convert temperature to Celsius
        celsius = kelvin - 273.15

        return celsius

def visualize_temperature_map(temperature_map, title="Temperature Map"):
    plt.imshow(temperature_map, cmap='hot', vmin=temperature_map.min(), vmax=temperature_map.max())
    plt.colorbar(label='Temperature (°C)')
    plt.title(title)
    plt.show()

def visualize_lst_map(lst_map, title="LST Map"):
    plt.imshow(lst_map, cmap='hot', vmin=lst_map.min(), vmax=lst_map.max())
    plt.colorbar(label='LST (°C)')
    plt.title(title)
    plt.show()

def main():
    # Replace these paths with the paths to your Landsat 8 TIR bands
    band10_path = 'path/to/LC08_L1TP_..._B10.tif'
    band11_path = 'path/to/LC08_L1TP_..._B11.tif'

    # Extract LST
    lst = extract_lst(band10_path, band11_path)

    # Visualize temperature map
    visualize_temperature_map(lst, title="Temperature Map")

    # You can also visualize the LST map if needed
    visualize_lst_map(lst, title="LST Map")


# Gaussian-Plume-Model
A Gaussian Plume model for simulating the dispersion of air pollutants in the atmosphere. The model calculates pollutant concentrations based on emission sources such as stacks. It is designed for both urban and rural environments.
def main():
    # Step 1: Input data from the user
    # Get the dimensions of the domain (length and width)
    O = float(input('Domain length (km) = '))
    W = float(input('Domain width (km) = '))

    # Get the dimensions of the receptor network (length, width, and height)
    M = float(input('Receptor network length (km) = '))
    N1 = float(input('Receptor network width (km)= '))
    z = float(input('Receptor network height (m)= '))

    # Get the pollutant to model (SO2, NO2, CO, PM, O3)
    pollutant = input('Enter the desired pollutant (SO2, NO2, CO, PM, O3): ')

    # Get the stability category (A to F) which defines the atmospheric conditions
    Stability_Category = input('Enter the type of Stability Category(A to F): ')

    # Get the height of the planetary boundary layer (H_m)
    H_m = float(input('Enter the height of the planetary boundary layer(m): '))

    # Get the area type (urban or rural)
    area_type = input('Enter area type (urban or rural): ')

    # Get the height of the anemometer (Z_ref), ambient temperature (T_a), and wind speed (u_ref) at the anemometer
    Z_ref = float(input('Enter the height of the ananometer(m): '))
    T_a = float(input('Enter the ambient temperature(K): '))
    u_ref = float(input('Enter the wind speed at the height of the anemometer (m/s): '))

    # Get the surface roughness parameters and temperature values for calculations
    Z0_m = float(input('Enter Z0,m: '))
    Z0_h = float(input('Enter Z0,h: '))
    θv_Zr = float(input('Enter θv_Zr: '))
    θv_Z0_h = float(input('Enter θv_Z0_h: '))
    z_d = float(input('Enter deposition reference height (m): '))

    # Get the number of stacks (pollution sources)
    N = int(input('Enter the number of stacks: '))

    # Step 2: Process data for each stack (pollution source)
    C = []  # List to store the concentration values for each stack
    for i in range(N):
        print(f'Enter information of stack #{i + 1}')
        
        # Get the position of the stack (x, y coordinates)
        x_ = float(input('x_ (km) = '))
        y_ = float(input('y_ (km) = '))
        
        # Get the parameters related to the stack (velocity, diameter, emission rate, stack height, etc.)
        v_s = float(input('Flue gas velocity (m/s): '))
        d = float(input('Stack inside diameter (m): '))
        Q = float(input('Emission rate (g/s): '))
        h_s = float(input('Stack height (m): '))
        T_s = float(input('Stack gas exit temperature (K): '))
        d_p = float(input('Particle diameter (µm): '))
        𝜌_𝑐 = float(input('Particle density (g/cm³): '))
        
        # Step 3: Calculate sigma_z (vertical dispersion) for the current stack
        sigma_z = calculate_sigma_z(x_, Stability_Category)
        
        # Step 4: Calculate the wind speed at the stack height (u_s)
        u_s = calculate_wind_speed(u_ref, h_s, Z_ref, 0.07)  # Use p = 0.07 as an example for rural areas
        
        # Step 5: Calculate the temperature change between stack gas and ambient temperature
        ΔT = calculate_temperature_change(T_s, T_a)
        
        # Step 6: Calculate the pollutant concentration at the receptor network based on the emission rate and wind speed
        concentration = calculate_concentration(Q, 1, 1, 1, sigma_z, u_s, 1)  # Assumed some parameters as simplified
        C.append(concentration)  # Store the concentration for this stack
    
    # Step 7: Calculate the total concentration over all stacks and receptor points
    total_concentration = calculate_total_concentration(C, N)
    
    # Step 8: Output the results
    for i, conc in enumerate(total_concentration):
        print(f'C {i} = {conc} μg')  # Display concentration for each receptor network point

# Execute the main function when the script is run
if __name__ == '__main__':
    main()

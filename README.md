
# BBVA Offices Geocoding and Proximity Analysis

This project is a Python-based tool that helps analyze the proximity of BBVA offices in Lima, Peru, to the addresses of group members. The script performs the following tasks:
1. Imports the data with BBVA office addresses.
2. Geocodes the BBVA offices using the Google Maps API to get latitude and longitude.
3. Calculates driving times from each group member's address to the BBVA offices using the Google Maps Directions API.
4. Reports on the closest and furthest BBVA offices for each member.

## Requirements

The script requires the following Python packages:
- `pandas`
- `googlemaps`
- `datetime`
- `re`
- `tabulate`

Install the required packages using `pip`:

```bash
pip install pandas googlemaps tabulate
```

You also need a valid Google Maps API key to use the geocoding and directions services. Replace the placeholder in the script with your own key.

## Steps

### 1. Import BBVA Office Data

The script starts by importing the list of BBVA offices in Lima from an Excel file.

```python
import pandas as pd
archivo_excel = '../../_data/bbva_list.xlsx'
df = pd.read_excel(archivo_excel)
```

### 2. Geocode the BBVA Offices

The script uses the Google Maps Geocoding API to retrieve latitude and longitude data for each BBVA office based on its full address.

```python
import googlemaps
gmaps = googlemaps.Client(key='YOUR_GOOGLE_MAPS_API_KEY')

df['Latitud'] = None
df['Longitud'] = None
# Geocode each office
for index, row in df.iterrows():
    geocode_result = gmaps.geocode(row['Direccion Completa'])
    if geocode_result:
        location = geocode_result[0]['geometry']['location']
        df.at[index, 'Latitud'] = location['lat']
        df.at[index, 'Longitud'] = location['lng']
```

### 3. Calculate Driving Times

The script calculates the driving time between each group member's address and every BBVA office using the Google Maps Directions API.

```python
for direccion_miembro in direcciones_miembros:
    for direccion_bbva in direcciones_bbva:
        directions_result = gmaps.directions(direccion_miembro, direccion_bbva, mode="driving", departure_time=datetime.now())
        # Extract and display the driving time
        if directions_result:
            duracion = directions_result[0]['legs'][0]['duration']['text']
```

### 4. Report Closest and Furthest Offices

The script calculates and stores information about the closest and furthest BBVA offices from each group member’s address. The results are displayed in a well-organized DataFrame.

```python
import pandas as pd
df = pd.DataFrame.from_dict(distances_info, orient='index')
df['nombre_persona'] = ['Josue', 'Isaí', 'Michael', 'Carmen', 'Claudia']
```

### Example Output

The script will output a summary of the closest and furthest BBVA offices for each group member, as well as the corresponding driving times.

```
+-------------------------+----------------+----------------+------------------------------------+------------------------------------+
|         Address          | Closest Office | Closest Time    | Furthest Office                    | Furthest Time                     |
+-------------------------+----------------+----------------+------------------------------------+------------------------------------+
| Av. Universitaria        | CRUCE AV. TOMAS VALLE | 10 mins | AV. 15 DE JULIO, ATE              | 64 mins                           |
| C. Chinchero 175         | CA. LOS ALAMOS        | 9 mins  | MALECON ANDRAS A. CACERES, VENTANILLA | 90 mins                           |
| ...                      | ...                  | ...      | ...                                | ...                               |
+-------------------------+----------------+----------------+------------------------------------+------------------------------------+
```

## Notes

- You will need a Google Maps API key to run this script. Visit [Google Cloud](https://console.cloud.google.com/) to create one.
- The script is designed to handle exceptions in case the Google Maps API doesn't return results for a specific address.
- Ensure your Excel file with BBVA addresses is formatted correctly for the geocoding process.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

Feel free to customize this README further to suit your needs!

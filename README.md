# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

### Date: 18/10/25
### Register_no: 212222110026

# Aim: 
Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights with Multiple AI Tools

**#AI Tools Required:**
<br>ChatGPT (OpenAI GPT-5)
<br>Google Gemini (Pro Vision / Gemini 1.5 Pro)

**#Explanation:**
<br>Experiment the persona pattern as a programmer for any specific applications related with your interesting area. 
<br>Generate the outoput using more than one AI tool and based on the code generation analyse and discussing that. 

**# Conclusion:**
**#TASK 1 :Prompt to Generate Python Code for Interacting with Multiple APIs**
<br>**Prompt:** <br>Act as a Python programmer. Write Python code that fetches current weather data for Chennai from two different APIs — OpenWeatherMap and WeatherAPI. Display the temperature, humidity, and condition from both APIs.
<br>Output (ChatGPT): <br>
```
import requests
# API Keys (replace with your own)
openweather_key = "YOUR_OPENWEATHER_KEY"
weatherapi_key = "YOUR_WEATHERAPI_KEY"
city = "Chennai"
# OpenWeatherMap API
url1 = f"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={openweather_key}&units=metric"
data1 = requests.get(url1).json()
# WeatherAPI
url2 = f"http://api.weatherapi.com/v1/current.json?key={weatherapi_key}&q={city}"
data2 = requests.get(url2).json()
# Extracting data
ow_temp = data1['main']['temp']
ow_humidity = data1['main']['humidity']
ow_condition = data1['weather'][0]['description']
wa_temp = data2['current']['temp_c']
wa_humidity = data2['current']['humidity']
wa_condition = data2['current']['condition']['text']
print("---- OpenWeatherMap ----")
print(f"Temperature: {ow_temp}°C, Humidity: {ow_humidity}%, Condition: {ow_condition}")
print("---- WeatherAPI ----")
print(f"Temperature: {wa_temp}°C, Humidity: {wa_humidity}%, Condition: {wa_condition}")
```

The code directly requests JSON data from both APIs, extracts the relevant weather values, and prints them. It’s compact and efficient but lacks exception handling.

<br>ChatGPT produced a simple, functional Python script that:
<br>1.Fetches weather data using requests from both APIs.
<br>2.Extracts key parameters — temperature, humidity, and condition.
<br>3.Prints results clearly for both APIs side by side.

**Output (Gemini): **
<br>
```
import requests
import json
# --- CONFIGURATION ---
# IMPORTANT: Replace these placeholders with your actual API keys.
OPENWEATHERMAP_API_KEY = "YOUR_OPENWEATHERMAP_API_KEY"
WEATHERAPI_API_KEY = "YOUR_WEATHERAPI_API_KEY"
CITY_NAME = "Chennai, IN" # Using "Chennai, IN" for better specificity
# --- API Endpoints ---
OWM_BASE_URL = "https://api.openweathermap.org/data/2.5/weather"
WA_BASE_URL = "http://api.weatherapi.com/v1/current.json"
def get_openweathermap_data(city, api_key):
    """Fetches and parses current weather data from OpenWeatherMap."""
    print("--- Fetching data from OpenWeatherMap ---")
    params = {
        "q": city,
        "appid": api_key,
        "units": "metric" # Requesting temperature in Celsius
    }
    try:
        response = requests.get(OWM_BASE_URL, params=params, timeout=10)
        response.raise_for_status() # Raise an HTTPError for bad responses (4xx or 5xx)
        data = response.json()
        # Check for successful API response structure
        if data.get("cod") != 200:
            print(f"Error from OWM API: {data.get('message', 'Unknown error')}")
            return None
        # Extract required metrics
        temp = data["main"]["temp"]
        humidity = data["main"]["humidity"]
        condition = data["weather"][0]["description"]
        return {
            "Temperature": f"{temp}°C",
            "Humidity": f"{humidity}%",
            "Condition": condition.capitalize()
        }
    except requests.exceptions.RequestException as e:
        print(f"Error fetching data from OpenWeatherMap: {e}")
        return None
    except KeyError as e:
        print(f"Error parsing OWM response (Missing key: {e}). Check JSON structure.")
        return None
    except json.JSONDecodeError:
        print("Error: OWM response could not be parsed as JSON.")
        return None
def get_weatherapi_data(city, api_key):
    """Fetches and parses current weather data from WeatherAPI."""
    print("--- Fetching data from WeatherAPI ---")
    params = {
        "key": api_key,
        "q": city
    }
    try:
        response = requests.get(WA_BASE_URL, params=params, timeout=10)
        response.raise_for_status() # Raise an HTTPError for bad responses (4xx or 5xx)
        data = response.json()
        # Check for API errors reported within the JSON body
        if "error" in data:
            print(f"Error from WeatherAPI: {data['error']['message']}")
            return None
        # Extract required metrics
        # The structure is data -> current -> temp_c, humidity, condition -> text
        temp = data["current"]["temp_c"]
        humidity = data["current"]["humidity"]
        condition = data["current"]["condition"]["text"]
        return {
            "Temperature": f"{temp}°C",
            "Humidity": f"{humidity}%",
            "Condition": condition
        }
    except requests.exceptions.RequestException as e:
        print(f"Error fetching data from WeatherAPI: {e}")
        return None
    except KeyError as e:
        print(f"Error parsing WeatherAPI response (Missing key: {e}). Check JSON structure.")
        return None
    except json.JSONDecodeError:
        print("Error: WeatherAPI response could not be parsed as JSON.")
        return None
def main():
    """Main function to execute API calls and display results."""
    if OPENWEATHERMAP_API_KEY == "YOUR_OPENWEATHERMAP_API_KEY" or WEATHERAPI_API_KEY == "YOUR_WEATHERAPI_API_KEY":
        print("!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
        print("ERROR: Please update 'OPENWEATHERMAP_API_KEY' and 'WEATHERAPI_API_KEY'")
        print("in the script with your actual API keys before running.")
        print("!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
        return
    print(f"\n--- Current Weather Check for {CITY_NAME} ---\n")
    # 1. Fetch OpenWeatherMap Data
    owm_weather = get_openweathermap_data(CITY_NAME, OPENWEATHERMAP_API_KEY)
    print("\n" + "="*40 + "\n")
    # 2. Fetch WeatherAPI Data
    wa_weather = get_weatherapi_data(CITY_NAME, WEATHERAPI_API_KEY)
    print("\n" + "="*40)
    print("--- CONSOLIDATED RESULTS ---")
    print("="*40)
    # 3. Display Results
    if owm_weather:
        print("\n[ OpenWeatherMap ]")
        print(f"  Temperature: {owm_weather['Temperature']}")
        print(f"  Humidity:    {owm_weather['Humidity']}")
        print(f"  Condition:   {owm_weather['Condition']}")
    else:
        print("\n[ OpenWeatherMap ] - Failed to retrieve data.")
    if wa_weather:
        print("\n[ WeatherAPI ]")
        print(f"  Temperature: {wa_weather['Temperature']}")
        print(f"  Humidity:    {wa_weather['Humidity']}")
        print(f"  Condition:   {wa_weather['Condition']}")
    else:
        print("\n[ WeatherAPI ] - Failed to retrieve data.")
    
    print("\n" + "="*40)
if __name__ == "__main__":
    main()
```
Gemini’s code uses modular functions, timeouts, exception handling, and better readability. It’s more suitable for production-level use and easier to extend for additional features.

<br>Gemini generated a more structured and robust code, including:
<br>1.Functions for both API requests (get_openweathermap_data() and get_weatherapi_data()).
<br>2.Error handling for connection issues and invalid API keys.
<br>3.Clear display of consolidated results with formatted outputs.


**#TASK 2: Prompt to Compare Outputs and Highlight Differences**

**Prompt:**<br> Now write a Python function that compares the results of the two APIs (temperature, humidity, and condition) and prints the differences clearly.

**Output (ChatGPT):** <br>
```
def compare_weather(ow, wa):
    temp_diff = abs(ow['temp'] - wa['temp'])
    hum_diff = abs(ow['humidity'] - wa['humidity'])
    print("\n--- Comparison Report ---")
    print(f"Temperature difference: {temp_diff}°C")
    print(f"Humidity difference: {hum_diff}%")
    if ow['condition'] != wa['condition']:
        print(f"Condition mismatch: {ow['condition']} vs {wa['condition']}")
    else:
        print("Both APIs report similar conditions.")
# Example data
ow = {'temp': 32.4, 'humidity': 70, 'condition': 'Cloudy'}
wa = {'temp': 31.9, 'humidity': 68, 'condition': 'Partly cloudy'}
compare_weather(ow, wa)
```
This version is simple and easy to understand, ideal for demonstrating concept-level comparison.

<br>ChatGPT’s function compare_weather():
<br>1.Calculates the absolute difference in temperature and humidity.
<br>2.Checks if conditions match or differ.
<br>3.Prints a short comparison report.



**Output ( Gemini): **
<br>

```
import requests
import json
from typing import Optional, Dict, Any
# --- CONFIGURATION ---
# IMPORTANT: Replace these placeholders with your actual API keys.
OPENWEATHERMAP_API_KEY = "YOUR_OPENWEATHERMAP_API_KEY"
WEATHERAPI_API_KEY = "YOUR_WEATHERAPI_API_KEY"
CITY_NAME = "Chennai, IN" # Using "Chennai, IN" for better specificity
# --- API Endpoints ---
OWM_BASE_URL = "https://api.openweathermap.org/data/2.5/weather"
WA_BASE_URL = "http://api.weatherapi.com/v1/current.json"
# Type definition for clear type hinting
WeatherResult = Optional[Dict[str, str]]
def get_openweathermap_data(city: str, api_key: str) -> WeatherResult:
    """Fetches and parses current weather data from OpenWeatherMap."""
    print("--- Fetching data from OpenWeatherMap ---")
    params = {
        "q": city,
        "appid": api_key,
        "units": "metric" # Requesting temperature in Celsius
    }
    try:
        # Use a timeout and check status immediately
        response = requests.get(OWM_BASE_URL, params=params, timeout=10)
        response.raise_for_status() # Raise an HTTPError for bad responses (4xx or 5xx)
        data = response.json()
        # Check for successful API response structure
        if data.get("cod") != 200:
            print(f"Error from OWM API: {data.get('message', 'Unknown error')}")
            return None
        # Extract required metrics
        # OpenWeatherMap gives temperature as float/int
        temp = data["main"]["temp"]
        humidity = data["main"]["humidity"]
        # Condition is in an array under the 'weather' key
        condition = data["weather"][0]["description"]
        return {
            "Temperature": f"{temp}°C",
            "Humidity": f"{humidity}%",
            "Condition": condition.capitalize()
        }
    except requests.exceptions.RequestException as e:
        print(f"Error fetching data from OpenWeatherMap: {e}")
        return None
    except KeyError as e:
        print(f"Error parsing OWM response (Missing key: {e}). Check JSON structure.")
        return None
    except json.JSONDecodeError:
        print("Error: OWM response could not be parsed as JSON.")
        return None
def get_weatherapi_data(city: str, api_key: str) -> WeatherResult:
    """Fetches and parses current weather data from WeatherAPI."""
    print("--- Fetching data from WeatherAPI ---")
    params = {
        "key": api_key,
        "q": city
    }
    try:
        # Use a timeout and check status immediately
        response = requests.get(WA_BASE_URL, params=params, timeout=10)
        response.raise_for_status() # Raise an HTTPError for bad responses (4xx or 5xx)
        data = response.json()
        # Check for API errors reported within the JSON body
        if "error" in data:
            print(f"Error from WeatherAPI: {data['error']['message']}")
            return None
        # Extract required metrics
        # WeatherAPI gives temperature in 'temp_c'
        temp = data["current"]["temp_c"]
        humidity = data["current"]["humidity"]
        # Condition is nested deeper under 'condition'
        condition = data["current"]["condition"]["text"]
        return {
            "Temperature": f"{temp}°C",
            "Humidity": f"{humidity}%",
            "Condition": condition
        }
    except requests.exceptions.RequestException as e:
        print(f"Error fetching data from WeatherAPI: {e}")
        return None
    except KeyError as e:
        print(f"Error parsing WeatherAPI response (Missing key: {e}). Check JSON structure.")
        return None
    except json.JSONDecodeError:
        print("Error: WeatherAPI response could not be parsed as JSON.")
        return None
def compare_weather_data(owm_data: WeatherResult, wa_data: WeatherResult):
    """Compares weather metrics from two different API sources and prints differences."""
    
    print("\n\n" + "#"*50)
    print("### DETAILED API COMPARISON REPORT ###")
    print("#"*50)
    
    # 1. Handle cases where one or both APIs failed
    if not owm_data and not wa_data:
        print("Comparison failed: Neither API returned valid data.")
        return
    
    if not owm_data:
        print("OpenWeatherMap data is unavailable. Cannot perform full comparison.")
        return
        
    if not wa_data:
        print("WeatherAPI data is unavailable. Cannot perform full comparison.")
        return
    # 2. Define metrics to compare and initialize tracking
    metrics = ["Temperature", "Humidity", "Condition"]
    differences_found = False
    # Print Header for comparison table
    print(f"\n{'Metric':<15} | {'OpenWeatherMap':<20} | {'WeatherAPI':<20} | Status")
    print("-" * 75)
    # 3. Iterate through metrics and compare
    for metric in metrics:
        owm_value = owm_data.get(metric, "N/A")
        wa_value = wa_data.get(metric, "N/A")
        
        # Check for difference. Comparison is done on string representation (e.g., "30.5°C")
        if owm_value != wa_value:
            status = "DIFFERENCE"
            differences_found = True
        else:
            status = "MATCH"
            
        print(f"{metric:<15} | {owm_value:<20} | {wa_value:<20} | {status}")
    # 4. Final Summary
    print("-" * 75)
    if differences_found:
        print("\n\u2757 Discrepancies found in one or more metrics.")
        print("Note: Minor differences in temperature/humidity are common due to different data sources.")
    else:
        print("\n\u2705 All key metrics match between the two API sources.")
def main():
    """Main function to execute API calls and display results."""
    if OPENWEATHERMAP_API_KEY == "YOUR_OPENWEATHERMAP_API_KEY" or WEATHERAPI_API_KEY == "YOUR_WEATHERAPI_API_KEY":
        print("!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
        print("ERROR: Please update 'OPENWEATHERMAP_API_KEY' and 'WEATHERAPI_API_KEY'")
        print("in the script with your actual API keys before running.")
        print("!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
        return
    print(f"\n--- Current Weather Check for {CITY_NAME} ---\n")
    # 1. Fetch OpenWeatherMap Data
    owm_weather = get_openweathermap_data(CITY_NAME, OPENWEATHERMAP_API_KEY)
    print("\n" + "="*40 + "\n")
    # 2. Fetch WeatherAPI Data
    wa_weather = get_weatherapi_data(CITY_NAME, WEATHERAPI_API_KEY)
    # 3. Display Consolidated Results
    print("\n" + "="*40)
    print("--- RAW API EXTRACTION ---")
    print("="*40)
    if owm_weather:
        print("\n[ OpenWeatherMap ]")
        print(f"  Temperature: {owm_weather['Temperature']}")
        print(f"  Humidity:    {owm_weather['Humidity']}")
        print(f"  Condition:   {owm_weather['Condition']}")
    else:
        print("\n[ OpenWeatherMap ] - Failed to retrieve data.")
    if wa_weather:
        print("\n[ WeatherAPI ]")
        print(f"  Temperature: {wa_weather['Temperature']}")
        print(f"  Humidity:    {wa_weather['Humidity']}")
        print(f"  Condition:   {wa_weather['Condition']}")
    else:
        print("\n[ WeatherAPI ] - Failed to retrieve data.")
    
    # 4. Run Comparison
    compare_weather_data(owm_weather, wa_weather)
    
    print("\n" + "="*40)
if __name__ == "__main__":
    main()
```
Gemini’s comparison code prints a well-structured report and manages error cases effectively. It provides better clarity for real-world data analysis.

<br>Gemini produced a more detailed script with:
<br>1.Comprehensive comparison using formatted tables.
<br>2.Data validation for missing values.
<br>3.A structured summary highlighting mismatches and matches across metrics.

**#TASK 3: Prompt to Generate Insights**

**Prompt:** <br> Based on the comparison results, write a Python snippet that prints suggestions — for example, which API seems more accurate or if the user should average both values for better reliability.

**Output (ChatGPT):**
<br>
```
def generate_insights(temp_diff, hum_diff):
    print("\n--- Insights ---")
    if temp_diff < 1 and hum_diff < 5:
        print("Both APIs are consistent. You can rely on either.")
    else:
        print("Noticeable variation detected. Averaging both readings is recommended.")
        print("Consider verifying with a third data source for higher accuracy.")
```

It’s a quick, simple decision-making function that prints a brief insight based on numerical differences.

<br>ChatGPT created a short function generate_insights() that:
<br>1.Suggests whether the APIs are consistent or not.
<br>2.Recommends averaging the values if significant differences are found.

**Output (Gemini):**
<br>
```
import requests
import json
from typing import Optional, Dict, Any
# --- CONFIGURATION ---
# IMPORTANT: Replace these placeholders with your actual API keys.
OPENWEATHERMAP_API_KEY = "YOUR_OPENWEATHERMAP_API_KEY"
WEATHERAPI_API_KEY = "YOUR_WEATHERAPI_API_KEY"
CITY_NAME = "Chennai, IN" # Using "Chennai, IN" for better specificity
# --- API Endpoints ---
OWM_BASE_URL = "https://api.openweathermap.org/data/2.5/weather"
WA_BASE_URL = "http://api.weatherapi.com/v1/current.json"
# Type definition for clear type hinting
WeatherResult = Optional[Dict[str, str]]
def extract_numeric_value(value_str: str) -> Optional[float]:
    """Helper function to strip units and extract the numeric part of a metric string (e.g., '30.5°C' -> 30.5)."""
    if not isinstance(value_str, str):
        return None
    try:
        # Filter keeps digits, decimal point, and negative sign
        numeric_part = ''.join(filter(lambda x: x.isdigit() or x == '.' or x == '-', value_str))
        if numeric_part:
            return float(numeric_part)
        return None
    except ValueError:
        return None
def get_openweathermap_data(city: str, api_key: str) -> WeatherResult:
    """Fetches and parses current weather data from OpenWeatherMap."""
    print("--- Fetching data from OpenWeatherMap ---")
    params = {
        "q": city,
        "appid": api_key,
        "units": "metric" # Requesting temperature in Celsius
    }
    try:
        response = requests.get(OWM_BASE_URL, params=params, timeout=10)
        response.raise_for_status()
        data = response.json()
        if data.get("cod") != 200:
            print(f"Error from OWM API: {data.get('message', 'Unknown error')}")
            return None
        temp = data["main"]["temp"]
        humidity = data["main"]["humidity"]
        condition = data["weather"][0]["description"]
        return {
            "Temperature": f"{temp}°C",
            "Humidity": f"{humidity}%",
            "Condition": condition.capitalize()
        }
    except requests.exceptions.RequestException as e:
        print(f"Error fetching data from OpenWeatherMap: {e}")
        return None
    except (KeyError, IndexError) as e:
        print(f"Error parsing OWM response (Missing key/index: {e}). Check JSON structure.")
        return None
    except json.JSONDecodeError:
        print("Error: OWM response could not be parsed as JSON.")
        return None
def get_weatherapi_data(city: str, api_key: str) -> WeatherResult:
    """Fetches and parses current weather data from WeatherAPI."""
    print("--- Fetching data from WeatherAPI ---")
    params = {
        "key": api_key,
        "q": city
    }
    try:
        response = requests.get(WA_BASE_URL, params=params, timeout=10)
        response.raise_for_status()
        data = response.json()
        if "error" in data:
            print(f"Error from WeatherAPI: {data['error']['message']}")
            return None
        temp = data["current"]["temp_c"]
        humidity = data["current"]["humidity"]
        condition = data["current"]["condition"]["text"]
        return {
            "Temperature": f"{temp}°C",
            "Humidity": f"{humidity}%",
            "Condition": condition
        }
    except requests.exceptions.RequestException as e:
        print(f"Error fetching data from WeatherAPI: {e}")
        return None
    except KeyError as e:
        print(f"Error parsing WeatherAPI response (Missing key: {e}). Check JSON structure.")
        return None
    except json.JSONDecodeError:
        print("Error: WeatherAPI response could not be parsed as JSON.")
        return None
def compare_weather_data(owm_data: WeatherResult, wa_data: WeatherResult):
    """Compares weather metrics from two different API sources and prints differences."""
    
    print("\n\n" + "#"*50)
    print("### DETAILED API COMPARISON REPORT ###")
    print("#"*50)
    
    if not owm_data and not wa_data:
        print("Comparison failed: Neither API returned valid data.")
        return False
    
    if not owm_data or not wa_data:
        print("One API failed to return data. Cannot perform full comparison.")
        return False
    metrics = ["Temperature", "Humidity", "Condition"]
    differences_found = False
    # Print Header for comparison table
    print(f"\n{'Metric':<15} | {'OpenWeatherMap':<20} | {'WeatherAPI':<20} | Status")
    print("-" * 75)
    # Iterate through metrics and compare
    for metric in metrics:
        owm_value = owm_data.get(metric, "N/A")
        wa_value = wa_data.get(metric, "N/A")
        
        if owm_value != wa_value:
            status = "DIFFERENCE"
            differences_found = True
        else:
            status = "MATCH"
            
        print(f"{metric:<15} | {owm_value:<20} | {wa_value:<20} | {status}")
    print("-" * 75)
    
    return differences_found
def give_suggestions(owm_data: WeatherResult, wa_data: WeatherResult):
    """Provides analytical suggestions based on the comparison results."""
    print("\n" + "="*50)
    print("### ANALYTICAL SUGGESTIONS ###")
    print("="*50)
    if not owm_data or not wa_data:
        print("Suggestion: Please ensure both APIs return data before making a reliability assessment.")
        return
    # --- 1. Compare Temperature ---
    owm_temp = extract_numeric_value(owm_data.get("Temperature", ""))
    wa_temp = extract_numeric_value(wa_data.get("Temperature", ""))
    
    temp_diff_threshold = 2.0 # Significant difference if temperature differs by more than 2.0°C
    
    if owm_temp is not None and wa_temp is not None:
        temp_difference = abs(owm_temp - wa_temp)
        
        if temp_difference >= temp_diff_threshold:
            avg_temp = (owm_temp + wa_temp) / 2
            print(f"TEMP ADVICE: The temperature difference is substantial ({temp_difference:.1f}°C).")
            print(f"Recommendation: Consider averaging the results. The consensus temperature is {avg_temp:.1f}°C.")
        elif temp_difference > 0:
            print(f"TEMP NOTE: The temperatures are slightly different, which is expected (different stations/models).")
            print("Action: Either is likely fine, but you could still use the average for robustness.")
        else:
            print("TEMP STATUS: Temperature readings match precisely or are very close.")
    else:
        print("TEMP ISSUE: Cannot compare temperature numerically due to parsing errors.")
    # --- 2. Compare Humidity ---
    owm_hum = extract_numeric_value(owm_data.get("Humidity", ""))
    wa_hum = extract_numeric_value(wa_data.get("Humidity", ""))
    
    hum_diff_threshold = 10.0 # Significant difference if humidity differs by more than 10%
    
    if owm_hum is not None and wa_hum is not None:
        hum_difference = abs(owm_hum - wa_hum)
        
        if hum_difference >= hum_diff_threshold:
            print(f"\nHUMIDITY ADVICE: The humidity readings differ by {hum_difference:.0f}%, which is quite large.")
            print("Recommendation: Check historical data for your location to see which API aligns better, or use the average.")
        elif hum_difference > 0:
            print("\nHUMIDITY NOTE: Minor differences in humidity (within 10%) are normal.")
        else:
            print("\nHUMIDITY STATUS: Humidity readings match perfectly.")
    else:
        print("\nHUMIDITY ISSUE: Cannot compare humidity numerically due to parsing errors.")
        
    # --- 3. Compare Condition (Qualitative) ---
    owm_cond = owm_data.get("Condition", "").lower()
    wa_cond = wa_data.get("Condition", "").lower()
    if owm_cond != wa_cond and owm_cond and wa_cond:
        print("\nCONDITION ADVICE: The descriptive conditions differ.")
        if "cloud" in owm_cond and ("sun" in wa_cond or "clear" in wa_cond):
            print("Observation: OpenWeatherMap might be reporting more cloud cover or a less specific description.")
            print("Recommendation: If visual accuracy is key, check a third source (like a local web camera).")
        else:
             print("Recommendation: Neither API is inherently more accurate for descriptions. Choose the one that provides the level of detail you prefer.")
    else:
        print("\nCONDITION STATUS: Weather descriptions match or are semantically very close.")
        
    print("="*50)
def main():
    """Main function to execute API calls and display results."""
    if OPENWEATHERMAP_API_KEY == "YOUR_OPENWEATHERMAP_API_KEY" or WEATHERAPI_API_KEY == "YOUR_WEATHERAPI_API_KEY":
        print("!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
        print("ERROR: Please update 'OPENWEATHERMAP_API_KEY' and 'WEATHERAPI_API_KEY'")
        print("in the script with your actual API keys before running.")
        print("!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
        return
    print(f"\n--- Current Weather Check for {CITY_NAME} ---\n")
    # 1. Fetch OpenWeatherMap Data
    owm_weather = get_openweathermap_data(CITY_NAME, OPENWEATHERMAP_API_KEY)
    print("\n" + "="*40 + "\n")
    # 2. Fetch WeatherAPI Data
    wa_weather = get_weatherapi_data(CITY_NAME, WEATHERAPI_API_KEY)
    # 3. Display Consolidated Results
    print("\n" + "="*40)
    print("--- RAW API EXTRACTION ---")
    print("="*40)
    if owm_weather:
        print("\n[ OpenWeatherMap ]")
        print(f"  Temperature: {owm_weather['Temperature']}")
        print(f"  Humidity:    {owm_weather['Humidity']}")
        print(f"  Condition:   {owm_weather['Condition']}")
    else:
        print("\n[ OpenWeatherMap ] - Failed to retrieve data.")
    if wa_weather:
        print("\n[ WeatherAPI ]")
        print(f"  Temperature: {wa_weather['Temperature']}")
        print(f"  Humidity:    {wa_weather['Humidity']}")
        print(f"  Condition:   {wa_weather['Condition']}")
    else:
        print("\n[ WeatherAPI ] - Failed to retrieve data.")
    
    # 4. Run Comparison
    comparison_successful = compare_weather_data(owm_weather, wa_weather)
    
    # 5. Run Suggestions
    if comparison_successful:
        give_suggestions(owm_weather, wa_weather)
    else:
        print("\nSkipping detailed suggestions as one or more API calls failed.")
    print("\n" + "="*40)
if __name__ == "__main__":
    main()
```
Gemini’s version combines numerical analysis, condition checking, and context-based recommendations, making it a realistic decision-support system.

<br>Gemini enhanced the concept by:
<br>1.Including helper functions to extract numeric values from strings (like “30°C”).
<br>2.Providing detailed analytical insights for temperature, humidity, and condition.
<br>3.Giving recommendations such as averaging values or checking historical accuracy.

**#Reflection Note:**

<br>The prompts designed for each stage were highly effective in guiding both AI tools to generate relevant and structured code.
<br>ChatGPT (GPT-5) provided quick and concise code suitable for basic understanding and prototyping.
<br>Google Gemini produced more detailed, production-ready code with strong error handling and formatted reports.
<br>This experiment showed how prompt clarity directly affects the complexity and quality of the AI-generated output.
<br>If refined further, the prompts could specify:
<br>The desired level of code complexity (e.g., beginner vs. professional).
<br>The output format (table, JSON, or text).
<br>The type of insights required (statistical, graphical, or advisory).

<br>Overall, both tools responded effectively, demonstrating that well-structured prompts lead to accurate, explainable, and executable code outputs across multiple AI platforms.

**# Result**: The corresponding Prompt is executed successfully.

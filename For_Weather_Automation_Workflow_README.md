## 🌤️ Weather Forecast Automation (n8n Workflow)
This n8n workflow automates the daily extraction, transformation, and storage of weather forecasts (specifically for Dhaka) from two different APIs. It runs twice daily (midnight and morning), processes forecast data, and logs it into a Google Sheet for analysis and reporting.

## 🧩 Features
- Scheduled weather data fetch from OpenWeatherMap and WeatherAPI
- Filters forecast data for only 00:00 and 09:00 times
- Merges and formats data for comparison
- Writes/updates weather data into a Google Sheet
- Handles fallback/default data if APIs fail

## 🔗 APIs Used
API	Endpoint	Note
- OpenWeatherMap	https://api.openweathermap.org/data/2.5/forecast?q={{ $json.City }}&appid=...&units=metric	Uses 5-day forecast API
- WeatherAPI	http://api.weatherapi.com/v1/forecast.json?key=...&q={{ $json.City }}&days=6	Uses hourly forecast (6 days)

## 🔐 Setup Instructions
1. 🔑 API Keys
Update the two HTTP Request nodes with your actual API keys:

OpenWeatherMap Node
Replace in URL: &appid=966e04510c00853f127f2a2912009ad1

✅ Where to update:
- Open HTTP Request Open Weather API node
- Replace appid value with your actual API key

WeatherAPI Node
Replace in URL: ?key=5359ae3bd110446e8db54901253105

✅ Where to update:
- Open HTTP Request1 Weather API node
- Replace key value with your actual WeatherAPI key

## 📊 Google Sheets Configuration
The workflow updates a Google Sheet with columns:

Column	Description
- Date	Forecast date
- Dhaka forecast API_1 (°C)	Temperature from OpenWeatherMap (formatted)
- Dhaka forecast API_2 (°C)	Temperature from WeatherAPI (formatted)

## ✅ Where to configure:
Open the Google Sheets and Google Sheets2 nodes

Update:
- Spreadsheet ID (documentId)
- Sheet Name (sheetName)
- Column mappings if your column names are different

## 📝 Adjust Column Names
If you want to rename or customize the column headers in your Google Sheet:
- Change the "schema" and "value" field mappings in the Google Sheets nodes.
- Match new column names with your sheet manually (or use n8n's UI mapping).

## 🕒 Schedule
The workflow runs at the following times:
- Midnight (00:00)
- Morning (09:00)

## Configured via a CRON expression in the Schedule Trigger node:
- 0 0 0,9 * * *     # Every day at 00:00 and 09:00

## ⚙️ Data Processing
- Filters only relevant timestamps (00:00 and 09:00) from each API.
- Merges the two data sources on datetime (dt_txt and time).
- Formats temperatures, adds degree symbols (°), and distinguishes between API_1 and API_2.
- Tags each entry as either midnight or morning (run_type).

## 🧪 Fallback Handling
If either API returns no data:
- The workflow will still output N/A temperature values for the next three days at 00:00 and 09:00.

## ✅ Example Output (in Google Sheets)
Date	API_1 (°C)	API_2 (°C)
2025-06-01	30°	29°
2025-06-02	31°	32°
2025-06-03	N/A	27°

##💡 Customization Tips
- 🌍 Change city: Update the Timezone string in the Code node to extract another city's name.
- 📅 Change time windows: Modify logic in Code nodes to filter different times (e.g., 12:00, 18:00).
- 📁 Change storage: Replace Google Sheets with Airtable, MySQL, or another destination node.

## 📁 File Overview

| Node Name        | Purpose                                           |
|------------------|---------------------------------------------------|
| Schedule Trigger | Runs workflow at scheduled times                  |
| HTTP Request (2) | Fetches forecast from both APIs                   |
| Code1 to Code7   | Filter, clean, transform, and merge data          |
| Google Sheets (2)| Writes/updates rows in Google Sheets              |
| Merge            | Joins both API results on timestamp               |
| If               | Directs different update paths by time group      |

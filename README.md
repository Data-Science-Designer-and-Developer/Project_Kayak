# 🌤️ **Kayak France - Weather & Hotel Recommendation Engine**
## CDSD Certification Project - Data Pipeline & Geospatial Analysis

## 📋 **Project Overview**
**CDSD Certification Deliverable**: Complete ETL pipeline building a **destination recommendation system** for French tourist cities combining **real-time weather forecasts** (Open-Meteo API) and **hotel availability** (Overpass API + Booking.com scraping).

**Business Value**: Help travelers choose optimal destinations based on **7-day weather scores** and **hotel density/quality**.

***

## 🎯 **Key Deliverables** ✅

| Deliverable | Status | Output |
|-------------|--------|--------|
| **S3 Data Lake** | ✅ Complete | `raw/weather.csv` + `raw/hotels.csv` |
| **RDS Data Warehouse** | ✅ Complete | `weather_cities` + `hotels_list` tables |
| **Interactive Maps** | ✅ Complete | Top-5 cities + Top-20 hotels (Plotly HTML) |
| **Production Pipeline** | ✅ Complete | Fully automated ETL with error handling |

***

## 🗺️ **Target Cities** (34 French destinations)
```
🏰 Mont Saint-Michel  🌊 Saint-Malo  🏛️ Bayeux  ⚓ Le Havre  🏰 Rouen
🎪 Paris  🕌 Amiens  🏗️ Lille  🥨 Strasbourg  ⛰️ Haut-Kœnigsbourg
🍷 Colmar  🏘️ Eguisheim  🏰 Besançon  🍷 Dijon  🏔️ Annecy
🏔️ Grenoble  🏙️ Lyon  🌊 Verdon  🌸 Bormes-les-Mimosas  🏖️ Cassis
🏙️ Marseille  🌿 Aix  🏛️ Avignon  🏰 Uzès  🏛️ Nîmes
🏰 Aigues-Mortes  🌊 Saintes-Maries  🎨 Collioure  🏰 Carcassonne
🏔️ Ariège  🏙️ Toulouse  🎪 Montauban  🏖️ Biarritz  ⚓ Bayonne  ⛵ La Rochelle
```

***

## 🛠️ **Technical Stack**

```yaml
🌐 APIs
├── Open-Meteo (weather geocoding + 7-day forecast)
├── Overpass-API (OpenStreetMap hotels)
└── Booking.com (scraping fallback)

☁️ Cloud
├── AWS S3 (Data Lake - raw/processed CSV)
├── AWS RDS Aurora PostgreSQL (Data Warehouse)
└── SSH tunnel (secure local-to-cloud DB access)

📊 Data Processing
├── pandas (ETL + cleaning)
├── requests/BeautifulSoup (web scraping)
├── sqlalchemy/psycopg2 (RDS connector)
└── plotly (interactive geospatial viz)

⚙️ Infrastructure
├── .env (secure credentials)
├── boto3 (AWS SDK)
├── Cache system (hotels_cache.json)
└── Retry logic + timeouts
```

***

## 🔄 **Production ETL Pipeline**

```mermaid
graph TD
    A[34 French Cities List] --> B[Geocoding<br>Open-Meteo API]
    B --> C[7-Day Weather Forecast<br/>nice_weather_score]
    C --> D[Hotel Search<br/>Overpass API<br/>10km radius]
    D --> E[S3 Data Lake<br/>weather.csv + hotels.csv]
    C --> F[RDS PostgreSQL<br/>weather_cities table]
    D --> G[RDS PostgreSQL<br/>hotels_list table]
    E --> H[Plotly Maps<br/>Top-5 Cities + Top-20 Hotels]
    F --> H
    G --> H
```

## 📊 **Key Features Implemented**

### **1. Robust Geocoding** 
```
✅ 34/34 cities geocoded successfully
✅ Special overrides (Mont Saint-Michel → Le Mont-Saint-Michel)
✅ Accent normalization + multiple fallback attempts
✅ Rate limiting (0.5s between requests)
```

### **2. Weather Scoring Algorithm**
```python
score = avg_temp_7days - (total_rain_mm * 0.1)
```
- **Higher score** = warmer + drier = better travel weather
- **Day 1→7 animated maps** (Plotly scatter_mapbox)

### **3. Hotel Intelligence**
```
✅ Primary: Overpass API ( OSM hotels within 10km )
✅ Fallback: Booking.com scraping (20 hotels/city)
✅ Cache system (hotels_cache.json)
✅ Data enrichment (stars, phone, website)
```

### **4. Cloud Infrastructure**
```
✅ S3 uploads: weather/french_cities_weather.csv
✅ RDS tables: weather_cities + hotels_list  
✅ SSH tunnel security (localhost:6544 → RDS:5432)
✅ Pre-flight connectivity checks
✅ Transactional SQL writes (engine.begin())
```

***

## 🗺️ **Interactive Visualizations Generated**

| Visualization | Description | Output File |
|---------------|-------------|-------------|
| **Top-5 Weather Cities** | Best destinations by nice_weather_score | `top5_cities.html` |
| **Top-20 Hotels** | Best hotels with weather context | `top20_hotels.html` |
| **Weather Animation** | Day 1→7 temperature evolution | `weather_forecast_animated.html` |
| **Filterable Map** | Temperature/Rain/Score toggle | `weather_filter_map.html` |
| **Combined Map** | Hotels overlaid on weather cities | `weather_hotels_map.html` |

***

## ☁️ **Cloud Deployment Status**

```
✅ S3 Bucket: dreipfeltbucketkayak
✅ RDS Cluster: database-1.cluster-cpmkokqkeqbb.eu-west-3.rds.amazonaws.com
✅ Tables created: weather_cities (34 rows), hotels_list (600+ rows)
✅ Data persistence: CSV + SQL backups
✅ Access: Secured via .env + IAM roles
```

**Connection String** (via SSH tunnel):
```
postgresql+psycopg2://postgres:[PASSWORD]@127.0.0.1:6544/postgres
```

***

## 🚀 **Production Pipeline** (One-Command Execution)

```python
df_geo, df_weather, df_hotels = main_pipeline()
# Automatically generates:
# ✅ CSV files
# ✅ S3 uploads  
# ✅ RDS tables
# ✅ 5 interactive Plotly maps
# ✅ hotels_cache.json (for future runs)
```

***

## 📈 **Sample Results** (Weather Scores)
```
1. 🌞 Aix-en-Provence: 24.8 (top score!)
2. 🏖️ Cassis: 23.1 
3. 🌿 Bormes-les-Mimosas: 22.7
4. 🏙️ Marseille: 22.3
5. 🎪 Paris: 18.2
...
34. 🏰 Mont Saint-Michel: 12.4
```

***

## 🔒 **Error Handling & Robustness**

```
✅ API timeouts + retry logic (3 attempts)
✅ Geocoding fallbacks (5 name variants)
✅ Cache system (avoid API hammering)
✅ Pre-flight connectivity checks
✅ Graceful degradation (missing data)
✅ Rate limiting (0.2-2s delays)
✅ Transactional DB writes
```

***

## 📋 **Certification Checklist** ✅

- [x] **External API Integration** (Open-Meteo, Overpass)
- [x] **Web Scraping** (Booking.com fallback)
- [x] **Cloud Storage** (S3 Data Lake)
- [x] **SQL Data Warehouse** (RDS PostgreSQL)
- [x] **Geospatial Visualizations** (Plotly Mapbox)
- [x] **Production Pipeline** (fully automated)
- [x] **Error Handling** (retries, caching, fallbacks)
- [x] **Documentation** (this README)

***

## 👨‍💻 **Author**
**[Dreipfelt]**  
*Data Science Student — CDSD Certification*  
**Portfolio**: [[GitHub](https://github.com/Dreipfelt?tab=repositories)] | **LinkedIn**: [LinkedIn]

***

## 🔗 **Usage**
```bash
# 1. Clone & setup
git clone <repo>
pip install -r requirements.txt

# 2. Configure (.env)
cp .env.example .env
# Edit AWS keys, RDS password, tunnel port

# 3. Launch SSH tunnel (separate terminal)
ssh -N -L 6544:<RDS_ENDPOINT>:5432 ec2-user@bastion

# 4. Run pipeline
jupyter notebook kayak_recommendation.ipynb
# Or: python main.py
```

**"Complete production-grade ETL pipeline: APIs → S3 → RDS → Interactive Dashboards"**# Project_Kayak

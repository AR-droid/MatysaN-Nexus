
---

 **NEMO – Navigational Engine Using Marine Optimization**

 *Transforming Coastal Livelihoods with AI-Powered Precision Fishing*

---

 Problem

Coastal fishermen in Tamil Nadu face declining fish stocks, unpredictable weather, and high fuel costs. Fishing decisions are driven by generational intuition rather than actionable data — leading to low yields, wasted trips, and financial vulnerability.



 Solution

**NEMO** is an AI-powered navigational assistant that uses real-time satellite data, historical fleet behavior, and marine regulations to recommend **high-yield, low-risk, and fuel-optimized fishing routes**, translated into spoken Tamil. Designed for zero-literacy, the platform offers intuitive audio and map guidance tailored to fishermen’s daily needs.


### ❖ Why It Matters

| Benefit                    | Impact                                                           |
| -------------------------- | ---------------------------------------------------------------- |
| **Data-Driven Fishing**    | Replaces intuition with environmental data and historical trends |
| **Fuel Savings**           | Route optimization reduces fuel consumption by up to 40%         |
| **Livelihood Upliftment**  | Improves catch predictability, income stability, and safety      |
| **Zero-Literacy Friendly** | Fully accessible with Tamil voice guidance                       |
| **Scalable & Affordable**  | Uses publicly available satellite and fleet data                 |

---

 Core Features

| Feature                   | Description                                                                                 |
| ------------------------- | ------------------------------------------------------------------------------------------- |
| **Fish Yield Prediction** | ML model trained on SST (Sea Surface Temperature), chlorophyll, and fleet movement data     |
| **Route Optimization**    | Distance-efficient and risk-averse algorithm suggesting best fishing path under fuel limits |
| **GFW Fleet Integration** | Upload fleet data (.csv) from Global Fishing Watch to visualize recent fishing activity     |
| **Tamil Audio Assistant** | Generates Tamil spoken instructions using gTTS                                              |
| **Risk Area Filtering**   | Filters zones with high waves, poor chlorophyll levels, or marine restrictions              |
| **Mobile Ready**          | Flask backend supports Android/iOS wrappers with lightweight UI                             |

---

 Tech Stack

| Layer                 | Tools & Libraries                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------ |
| **Backend**           | Python + Flask                                                                                   |
| **Frontend Map**      | Folium, Leaflet.js                                                                               |
| **AI Models**         | Scikit-learn, XGBoost, Custom LSTM models                                                        |
| **Optimization**      | NumPy, SciPy, Geopy, NetworkX                                                                    |
| **Voice Engine**      | gTTS (Tamil text-to-speech)                                                                      |
| **Data Sources**      | NASA (SST), NOAA (Chlorophyll), GFW (Fleet .csv uploads)                                         |
| **ML Chatbot (R\&D)** | Transformer from scratch (PyTorch), fine-tuned on Tamil marine FAQs for onboard query resolution |

---

 Model Training

Dataset

* **Fleet Behavior Data** – GFW `.csv` (3 datasets, merged and cleaned)
* **Satellite SST/Chlorophyll** – Extracted from open APIs & historical NetCDF files (converted)
* **Labels** – Approximate catch scores created from fleet density + SST alignment

 ML Pipeline

1. **Preprocessing** – Normalize SST, interpolate missing chlorophyll
2. **Feature Engineering** – Compute rolling averages, chlorophyll-to-SST delta
3. **Model Used**

   * **Regression:** XGBoost Regressor for catch probability
   * **Time-Series:** Custom LSTM for seasonal prediction
   * **Clustering:** DBSCAN for identifying high-yield zones based on grid density
4. **Fine-tuning**

   * Trained on temporal slices (month-wise)
   * Grid Search for hyperparameters

---

 Transformer Model (AI Chatbot – R\&D)

* Built using PyTorch Transformer from scratch
* Trained on FAQs in Tamil related to:

  * Fishing zones
  * Regulations
  * Weather conditions
* Preprocessing: Tokenized Tamil corpus, positional encoding
* Output: Tamil chatbot that can answer “இன்று கடல் நிலை எப்படி?”
<img width="607" height="339" alt="Screenshot 2025-07-18 at 8 24 39 AM" src="https://github.com/user-attachments/assets/2c93ba86-9518-4117-a4c8-a349fc9fc86b" />

  
<img width="656" height="215" alt="Screenshot 2025-07-18 at 8 25 12 AM" src="https://github.com/user-attachments/assets/d6a04d67-2473-4dbe-923e-7e2c2b256cdc" />

<img width="739" height="285" alt="Screenshot 2025-07-18 at 10 07 29 AM" src="https://github.com/user-attachments/assets/8e7ab274-2b68-4c54-8e9a-db0cebe5a843" />
<img width="756" height="231" alt="Screenshot 2025-07-18 at 10 07 52 AM" src="https://github.com/user-attachments/assets/c29f431f-dc9e-462e-9486-853dd5d824c5" />

---

 Optimization Logic

1. **Input:**

   * Current location (`lat/lon`)
   * Fuel limit (converted to distance range)
   * Latest fleet `.csv` data

2. Process:

   * Generate 10km grid from current location
   * Score each grid using:

     * ML-predicted yield
     * Distance from current location
     * Exclusion if in risk zones

3. Selection:

   * Apply yield-to-fuel ratio optimization
   * Compute safest shortest route using NetworkX
   * Validate against marine boundary constraints

4. Output:

   * Map with markers and Tamil instructions
   * Route line from current to optimal point
   * Alerts: red markers for restricted zones

---

 Example: Use Case

> A fisherman from Nagapattinam opens the app with a 30 km fuel range.

* The app scans uploaded `.csv` of recent fleet data

* SST + chlorophyll pattern suggests zone 22 km east with a **yield score of 0.91**

* Tamil voice instruction:

  > “மீனவர்கள் சகோதரரே, கிழக்கு திசைக்கு 22 கி.மீ பயணியுங்கள். அதிகமான மீன் வாய்ப்பு உள்ளது.”

* He returns with 3× higher catch, avoiding poor yield zones and high-risk waters.

---

###  APIs Used

| Purpose                           | API/Source                         |
| --------------------------------- | ---------------------------------- |
| **Sea Surface Temperature (SST)** | NASA GHRSST L4 MUR SST             |
| **Chlorophyll Data**              | NOAA CoastWatch                    |
| **Weather & Wind**                | OpenWeatherMap Marine API          |
| **Fleet Movement**                | Global Fishing Watch `.csv` Upload |
| **Geocoding/Distance**            | OpenStreetMap Nominatim, Geopy     |

---

###  How To Run

1. **Clone the Repository**

```bash
git clone https://github.com/your-repo/nemo-ai
cd nemo-ai
```

2. **Setup Virtual Environment**

```bash
python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

3. **Run the Flask App**

```bash
python app.py
```



4. **Enter Location & Fuel Range → Get Map & Tamil Voice Route**

---

### ❖ What’s Next

| Task                             | Status                                                        |
| -------------------------------- | ------------------------------------------------------------- |
| **Live Satellite Integration**   | Partially Done – Currently uses static + API blend            |
| **End-to-End Mobile Deployment** | Pending – Requires Android/iOS wrapper                        |
| **Tamil Chatbot UI**             | R\&D working prototype exists                                 |
| **Model Performance Dashboard**  | In Progress – Will show route efficiency and model accuracy   |
| **Onboard Device Integration**   | Future Scope – Raspberry Pi with GPS module for offline usage |

---
<img width="845" height="701" alt="Screenshot 2025-07-18 at 10 05 56 AM" src="https://github.com/user-attachments/assets/4d313215-24ed-466f-b987-7fedf0f1ddd6" />

UI SCREENSHOT
<img width="1459" height="751" alt="Screenshot 2025-07-18 at 10 08 43 AM" src="https://github.com/user-attachments/assets/4b005d74-897b-466c-80da-2346ef55cb77" /><img width="1165" height="560" alt="Screenshot 2025-07-18 at 10 09 11 AM" src="https://github.com/user-attachments/assets/0d1161c6-13ee-45fb-bb20-6e35451157cd" />
Working

https://youtu.be/PychHTBrZaI

### 📬 Contact

> **For queries or collaborations**
> anjanarangarajan06@gmail.com

---

### 🚀 TEAM NEXUS

* **Anjana Rangarajan**
* **Krishna Tulasi S**

---





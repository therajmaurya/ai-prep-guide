# Domain Knowledge for AI Engineers

As a Senior AI Engineer, technical skills (coding, algorithms) are only half the equation. The other half is **Domain Knowledge**—understanding the specific business context, data nuances, and KPIs of the industry you are working in. This guide covers the critical concepts, metrics, and AI use cases for key sectors.

---

## 1. Healthcare & Life Sciences
*Focus: Patient outcomes, privacy (HIPAA/GDPR), precision, and interpretability.*

### Core Domain Concepts
*   **Electronic Health Records (EHR)**: Digital version of patient charts.
*   **PACS (Picture Archiving and Communication System)**: Medical imaging storage.
*   **Clinical Trials**: Phases (I-IV) of testing new drugs.
*   **Phenotype vs. Genotype**: Observable traits vs. genetic makeup.
*   **False Negatives (Type II Error)**: Critical to minimize in screening (e.g., missing a cancer diagnosis is worse than a false alarm).

### Key Metrics/KPIs
*   **Sensitivity (Recall)**: Crucial for disease screening.
*   **Specificity**: avoiding false alarms.
*   **PPV/NPV (Positive/Negative Predictive Value)**.
*   **Readmission Rate**: Hospital quality metric.
*   **Drug Discovery Cycle Time**.

### Data Specificities
*   **DICOM**: Standard format for medical images (MRI, CT, X-ray).
*   **HL7/FHIR**: Standards for exchanging healthcare information.
*   **High Dimensionality**: Genomics data ($p \gg n$).
*   **Censored Data**: In survival analysis (patients leaving a study).

### Common AI Use Cases
1.  **Medical Imaging Diagnosis**: CNNs/Vision Transformers for detecting tumors in X-rays/MRIs.
2.  **Drug Discovery**: GNNs (Graph Neural Networks) for molecular property prediction; GenAI for protein folding (AlphaFold).
3.  **Clinical NLP**: Extracting symptoms and codes (ICD-10) from unstructured doctor's notes (BERT, LLMs).
4.  **Survival Analysis**: Predicting time-to-event (e.g., patient survival) using Cox Proportional Hazards or DeepSurv.

---

## 2. Finance (FinTech)
*Focus: Risk management, latency, regulatory compliance, and time-series accuracy.*

### Core Domain Concepts
*   **KYC (Know Your Customer) / AML (Anti-Money Laundering)**.
*   **Credit Scoring**: FICO scores, probability of default.
*   **High-Frequency Trading (HFT)**: Microsecond-level trading.
*   **Underwriting**: Assessing risk for insurance or loans.
*   **Alpha vs. Beta**: Active return vs. market return.

### Key Metrics/KPIs
*   **Sharpe Ratio**: Risk-adjusted return.
*   **Maximum Drawdown (MDD)**: Downside risk.
*   **Gini Coefficient / AUC-ROC**: For credit scoring models.
*   **False Positive Rate**: In fraud detection (blocking legitimate users is costly).

### Data Specificities
*   **Time-Series**: Non-stationary, noisy financial data (OHLCV - Open, High, Low, Close, Volume).
*   **Tabular Data**: Heavy use of structured data (transactions, demographics).
*   **Alternative Data**: Satellite imagery, social media sentiment for trading signals.

### Common AI Use Cases
1.  **Fraud Detection**: Anomaly detection (Isolation Forests, Autoencoders) and graph networks for money laundering rings.
2.  **Algorithmic Trading**: Reinforcement Learning (RL) and LSTM/Transformers for price prediction.
3.  **Credit Risk Modeling**: XGBoost/LightGBM for default prediction (interpretable AI is key here due to regulations).
4.  **Robo-Advising**: Personalized portfolio optimization.

---

## 3. Retail & E-commerce
*Focus: Personalization, inventory management, and customer lifetime value.*

### Core Domain Concepts
*   **SKU (Stock Keeping Unit)**: Unique identifier for products.
*   **Funnel Analysis**: Impressions -> Clicks -> Add to Cart -> Purchase.
*   **Omnichannel**: Integrating online and offline (brick-and-mortar) experiences.
*   **Churn**: Customers stopping business with the entity.

### Key Metrics/KPIs
*   **GMV (Gross Merchandise Value)**: Total sales volume.
*   **CLTV (Customer Lifetime Value)**: Predicted net profit from a future relationship.
*   **CAC (Customer Acquisition Cost)**.
*   **AOV (Average Order Value)**.
*   **Conversion Rate (CVR)**.

### Data Specificities
*   **Clickstream Data**: User session logs.
*   **Sparse Matrices**: User-Item interaction matrices for collaborative filtering.
*   **Seasonality**: Strong temporal patterns (Black Friday, Christmas).

### Common AI Use Cases
1.  **Recommendation Systems**: Collaborative Filtering, Matrix Factorization, Two-Tower Architectures (retrieval + ranking).
2.  **Dynamic Pricing**: Reinforcement Learning for price optimization based on demand.
3.  **Demand Forecasting**: Predicting inventory needs using Prophet, ARIMA, or deep learning (TFT - Temporal Fusion Transformers).
4.  **Visual Search**: Finding products by uploading an image (Siamese Networks, Embeddings).

---

## 4. Advertising Technology (AdTech)
*Focus: Real-time decision making, large scale data, and privacy (cookies deprecation).*

### Core Domain Concepts
*   **RTB (Real-Time Bidding)**: Auction happening in milliseconds when a page loads.
*   **DSP (Demand Side Platform)** vs **SSP (Supply Side Platform)**.
*   **Attribution**: Assigning credit to touchpoints (First-click, Last-click, Multi-touch).
*   **Programmatic Advertising**.

### Key Metrics/KPIs
*   **CTR (Click-Through Rate)**.
*   **CPM (Cost Per Mille)**: Cost per 1,000 impressions.
*   **eCPM (Effective Cost Per Mille)**.
*   **CPA (Cost Per Action)**.
*   **ROAS (Return on Ad Spend)**.

### Data Specificities
*   **High Cardinality**: Millions of websites, apps, and user IDs.
*   **Imbalanced Classes**: Clicks are rare events (CTR ~0.1% - 1%).
*   **Latency Constraints**: Inference must happen in <20ms.

### Common AI Use Cases
1.  **CTR Prediction**: Deep Learning Recommendation Models (DLRM), Wide & Deep Networks.
2.  **Bid Optimization**: Determining the optimal price to bid in an auction.
3.  **Audience Segmentation**: Clustering users based on behavior for targeting.

---

## 5. Supply Chain & Logistics
*Focus: Efficiency, route optimization, and resilience.*

### Core Domain Concepts
*   **Last-Mile Delivery**: The final step of the delivery process.
*   **JIT (Just-In-Time)**: Inventory strategy.
*   **Cold Chain**: Temperature-controlled supply chain.
*   **3PL (Third-Party Logistics)**.

### Key Metrics/KPIs
*   **OTIF (On-Time In-Full)**.
*   **Inventory Turnover**.
*   **Lead Time**.
*   **Fuel Consumption**.

### Data Specificities
*   **Geospatial Data**: GPS coordinates, routes.
*   **Graph Data**: Supply chain networks.

### Common AI Use Cases
1.  **Route Optimization**: Traveling Salesman Problem (TSP) variations using OR (Operations Research) and RL.
2.  **Demand Forecasting**: Predicting stock requirements at different warehouses.
3.  **Warehouse Automation**: Computer vision for package sorting and defect detection.
4.  **Digital Twin**: Simulating supply chain shocks.

---

## 6. Automotive & Autonomous Systems
*Focus: Safety, perception, and real-time processing.*

### Core Domain Concepts
*   **ADAS (Advanced Driver-Assistance Systems)**: Lane keeping, adaptive cruise control.
*   **SAE Levels**: Level 0 (No automation) to Level 5 (Full automation).
*   **V2X (Vehicle-to-Everything)**: Communication between vehicle and infrastructure.
*   **Sensor Fusion**: Combining data from Lidar, Radar, and Cameras.

### Key Metrics/KPIs
*   **mAP (Mean Average Precision)**: Object detection accuracy.
*   **IoU (Intersection over Union)**.
*   **Disengagement Rate**: How often humans must take over control.
*   **Latency**: Inference speed.

### Data Specificities
*   **Point Clouds**: 3D data from Lidar.
*   **Video Streams**: Large volume, temporal correlation.
*   **Edge Computing**: Models must run on-device (e.g., NVIDIA Jetson).

### Common AI Use Cases
1.  **Perception stack**: Object detection (YOLO, coarser R-CNN), Semantic Segmentation (UNet).
2.  **Path Planning**: Reinforcement Learning and A* search.
3.  **Predictive Maintenance**: analyzing engine sound/vibration to predict failure.
4.  **Driver Monitoring Systems (DMS)**: Detecting drowsiness via eye-tracking.

---

## 7. Manufacturing (Industry 4.0)
*Focus: Quality control, predictive maintenance, and operational efficiency.*

### Core Domain Concepts
*   **IoT (Internet of Things)** / IIoT (Industrial IoT).
*   **OEE (Overall Equipment Effectiveness)**.
*   **Six Sigma**: Quality improvement methodology.
*   **PLC (Programmable Logic Controller)**.

### Key Metrics/KPIs
*   **Yield Rate**: Percentage of non-defective items.
*   **Downtime**: Unplanned machine stoppage.
*   **MTBF (Mean Time Between Failures)**.

### Data Specificities
*   **Telemetric Data**: Vibration, temperature, pressure sensors.
*   **Time-Series**: High frequency (kHz).

### Common AI Use Cases
1.  **Visual Quality Inspection**: Detecting defects on assembly lines using Computer Vision.
2.  **Predictive Maintenance**: Remaining Useful Life (RUL) estimation using LSTMs/Transformers.
3.  **Process Optimization**: Reinforcement Learning for tuning machine parameters.

---

## 8. Energy & Utilities
*Focus: Reliability, forecasting, and grid optimization.*

### Core Domain Concepts
*   **Grid Balancing**: Matching supply and demand.
*   **Renewables Integration**: Solar/Wind intermittency.
*   **Smart Meters**.

### Key Metrics/KPIs
*   **MAPE (Mean Absolute Percentage Error)**: For load forecasting.
*   **LCOE (Levelized Cost of Energy)**.

### Common AI Use Cases
1.  **Load Forecasting**: Predicting energy demand.
2.  **Grid Optimization**: Smart grid management.
3.  **Asset Management**: Drone inspection of power lines.

---

## 9. Media & Entertainment
*Focus: Engagement, content creation, and personalization.*

### Core Domain Concepts
*   **Content Rights & Licensing**.
*   **Metadata**: Genres, actors, tags.
*   **User Engagement**: Watch time, binge-watching behavior.

### Key Metrics/KPIs
*   **Retention Rate**.
*   **AVOD (Ad-based VOD) / SVOD (Subscription VOD)** revenues.

### Common AI Use Cases
1.  **Personalized Recommendations**: Netflix/Spotify style engines.
2.  **Content Generation**: GenAI for scripts, music, or video (e.g., Sora, Runway).
3.  **Video Summarization**: Creating highlights automatically.
4.  **Dubbing/Subtitling**: Automatic speech recognition (ASR) and translation.

---

## 10. Cybersecurity
*Focus: Threat detection, zero-day attacks, and anomaly identification.*

### Core Domain Concepts
*   **Zero Trust Architecture**.
*   **SIEM (Security Information and Event Management)**.
*   **Phishing / Malware / Ransomware**.
*   **SOC (Security Operations Center)**.

### Key Metrics/KPIs
*   **MTTD (Mean Time To Detect)**.
*   **MTTR (Mean Time To Respond)**.
*   **False Positive Rate**: Critical to avoid alert fatigue.

### Common AI Use Cases
1.  **Network Intrusion Detection**: Inspecting packet flows for anomalies.
2.  **User Entity Behavior Analytics (UEBA)**: Baseling user behavior to detect compromised accounts.
3.  **Phishing Detection**: NLP analysis of emails.
4.  **Malware Classification**: Static and dynamic analysis of binaries.


### Recommended Sectors for Domain Knowledge
Healthcare & Life Sciences
Key Use Cases: Medical Imaging (Computer Vision), Drug Discovery (Generative AI), Personalized Medicine, Electronic Health Records (NLP).

Finance (FinTech)
Key Use Cases: Fraud Detection, Algorithmic Trading, Credit Risk Scoring, Customer Service Chatbots.

E-commerce & Retail
Key Use Cases: Recommendation Systems, Dynamic Pricing, Supply Chain Optimization, Visual Search.

Automotive & Autonomous Systems
Key Use Cases: Self-driving technology (Perception, Planning), Predictive Maintenance, Fleet Management.

Manufacturing & Industry 4.0
Key Use Cases: Predictive Maintenance, Quality Control (Computer Vision), Digital Twins, Robotics.

Media, Entertainment & Gaming
Key Use Cases: Content Generation (GenAI), Personalized Streaming Recommendations, Game AI, VFX.

Cybersecurity
Key Use Cases: Anomaly Detection, Phishing Detection, Threat Intelligence, Biometrics.

AdTech (Advertising Technology)
Key Use Cases: Real-time Bidding (RTB), Click-Through Rate (CTR) Prediction, User Profiling.

Legal & Compliance (LegalTech)
Key Use Cases: Contract Analysis (NLP), Case Prediction, Regulatory Compliance Automation.

Agriculture (AgriTech)
Key Use Cases: Crop Monitoring (Satellite Imagery), Precision Farming, Yield Prediction.

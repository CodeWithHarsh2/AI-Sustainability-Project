# 🌍 AI for Sustainability – Delhi Land Use Classification (SRIP 2026)

## 📌 Project Overview

This project was completed as part of the **AI for Sustainability – SRIP 2026 assignment**.

The main goal of this project is simple:

We use satellite images of the Delhi NCR region and automatically classify the land into categories such as:

- Built-up  
- Vegetation  
- Cropland  
- Water  
- Others  

This project builds a complete pipeline from start to finish:

1. Spatial filtering of satellite images  
2. Label generation using ESA WorldCover raster  
3. Training a CNN model  
4. Evaluating model performance  

The focus was to implement everything clearly and systematically, not just train a model.

---

## 📂 Dataset Provided

The following files were provided in the assignment:

- `Delhi_NCR_region.geojson` → Boundary of Delhi NCR  
- `Delhi_airshed.geojson` → Airshed boundary  
- `worldcover_bbox_Delhi_NCR_2021.tif` → ESA WorldCover 2021 land cover raster  
- `RGB/` folder → 9,216 Sentinel-2 RGB image patches (128 × 128 pixels, 10m resolution)

Each RGB image filename contains its center latitude and longitude coordinates.  
These coordinates are used for spatial filtering and label extraction.

---

## 🗂 Project Structure

```
AI_for_Sustainability_SRP_2026/
│
├── data/
│   ├── raw/
│   ├── processed/
│
├── notebooks/
│   └── main_pipeline.ipynb
│
├── outputs/
│   ├── figures/
│   ├── models/
│
├── requirements.txt
├── README.md
└── .gitignore
```

**Folder Explanation:**

- `data/raw/` → Original dataset files (not modified)  
- `notebooks/` → Main implementation notebook  
- `outputs/figures/` → Generated plots  
- `outputs/models/` → Saved trained model  

---

## 🧭 Step 1 – Spatial Reasoning & Filtering

We first load the Delhi NCR boundary using GeoPandas.

Then we:

- Convert CRS to EPSG:32644 (required for 60km gridding)
- Create a 60 km × 60 km uniform grid
- Overlay the grid on the Delhi NCR region
- Save the grid visualization

Next, we:

- Extract latitude and longitude from each image filename
- Convert them into spatial points
- Filter and keep only those images that fall inside the Delhi NCR boundary

We report:

- Total images before filtering  
- Total images after filtering  

---

## 🌾 Step 2 – Label Construction

Each RGB image is 128 × 128 pixels at 10m resolution.

For each filtered image:

1. Open the ESA WorldCover raster  
2. Extract the corresponding 128 × 128 land cover patch  
3. Count pixel class frequencies  
4. Assign the dominant (most frequent) class as the image label  

ESA classes are simplified into 5 categories:

| ESA Code | Simplified Label |
|----------|-----------------|
| 10,20,30 | Vegetation     |
| 40       | Cropland       |
| 50       | Built-up       |
| 80,90    | Water          |
| Others   | Others         |

After labeling:

- Perform 60/40 train-test split  
- Visualize class distribution  

---

## 🧠 Step 3 – CNN Model Training

We use a pretrained **ResNet18** model.

Steps:

- Replace final layer to match number of land-use classes  
- Use CrossEntropy Loss  
- Use Adam optimizer  
- Train for 5 epochs  

The model learns to classify satellite image patches into simplified land-use categories.

---

## 📊 Step 4 – Model Evaluation

We evaluate performance using:

- Accuracy  
- F1 Score  
- Confusion Matrix  

The confusion matrix helps understand which classes are predicted correctly and which classes are confused.

---

## 💾 Model Saving

After training, the model is saved as:

```
outputs/models/resnet18_landuse.pth
```

---

## ▶️ How to Run This Project

### 1. Clone the Repository

```
git clone <your-repository-link>
cd AI_for_Sustainability_SRP_2026
```

### 2. Create Virtual Environment

```
python -m venv venv
```

Activate:

Windows:
```
venv\Scripts\activate
```

Mac/Linux:
```
source venv/bin/activate
```

### 3. Install Dependencies

```
pip install -r requirements.txt
```

### 4. Place Dataset

Put all dataset files inside:

```
data/raw/
```

### 5. Run Notebook

Open:

```
notebooks/main_pipeline.ipynb
```

Run all cells sequentially.

---

## 🧩 What This Project Demonstrates

This project demonstrates understanding of:

- Coordinate reference systems (CRS)
- Spatial filtering and geospatial reasoning
- Raster data processing
- Land cover classification logic
- Deep learning with CNNs
- Model evaluation using proper metrics

It combines GIS, Remote Sensing, and Machine Learning into a complete working pipeline.

This project was built carefully step by step, focusing on clarity, structure, and correctness.

The objective was not only to obtain results, but to build a complete and understandable geospatial AI workflow from beginning to end.

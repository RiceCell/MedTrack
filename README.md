# MedTrack: Medicine Stockout Early Warning System
**Medicine stockout early warning synthetic data generator and analysis pipeline | A Project Built for Eskwelabs**

## 🏥 Project Overview
MedTrack is a highly realistic, mathematically grounded synthetic data generator simulating the Philippine Department of Health's (DOH) public supply chain. 

Built to train data professionals without exposing confidential patient or facility data, this project models the flow of tracer medicines (Paracetamol, TB Meds, Epinephrine) from central warehouses down to Rural Health Units (RHUs) and Barangay Health Stations (BHS). 

## Key Technical Features
* **Explicit Mathematical Modeling:** Demand is modeled using Poisson distributions, and supply chain delays are modeled using Exponential distributions, backed by real-world public health literature.
* **Vectorized Time-Series Generation:** Uses Pandas and NumPy vectorization to simulate daily inventory ledger deductions across thousands of facilities efficiently.
* **Omitted Variable Bias:** The simulation includes hidden rules—like `latent_management_quality` and seasonal `latent_outbreak_intensity` that dictate missing stock and demand spikes. These variables are removed before export, leaving behind realistic "messy" data (phantom stock, delivery delays) for analysts to grapple with.

## Data Architecture
The generator produces a relational database consisting of 5 tables:
1. `facilities.csv`: Static dimensions of RHUs and BHS clinics.
2. `deliveries.csv`: A supply chain log with realistic transit delays.
3. `consumption.csv`: Daily patient dispensing records driven by population and hidden epidemiological shocks.
4. `inventory_levels.csv`: End-of-month stock card audits.
5. `stockout_events.csv`: The ground-truth log of when physical inventory hit zero.

## Repository Structure
* `ERD_Eskwelabs_1.png`: The entity relationship diagram of all the relations for this synthetic dataset.
* `01_synthetic_generator.ipynb`: The Python script that configures and generates the raw data using NumPy and Pandas.
* `02_EDA_analysis.ipynb`: The analytical notebook proving the statistical realism of the data and featuring a predictive Decision Tree model.
* `SyntheticDataset/`: Folder containing the 5 generated CSV files.

## Key Analytical Insights
In **Notebook 2**, we trained a Scikit-Learn Decision Tree to predict whether a clinic would stock out of medicine in the next 30 days based on their monthly inventory audit. 

**The result?** The model achieved a high Recall (97%) but struggled with Precision (63%). This perfectly demonstrated the impact of real-world missing data: because the algorithm could not see the hidden `management_quality` of the clinics, it was fooled by the "phantom stock" on the ledgers, which proved that a model can only be as good as the reality captured in its dataset.

## How to Run
1. Clone this repository.
2. Install requirements: `pip install pandas numpy matplotlib seaborn scikit-learn`
3. Run `Notebook_1` to generate a fresh batch of synthetic data.
4. Run `Notebook_2` to visualize the seasonal outbreaks and train the Machine Learning model.

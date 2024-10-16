# UROP-2024-CFSbias
Using Machine Learning for Bias Correction in Weather Forecasts in the Great Lakes Region

## Project Background
The Great Lakes basin supports approximately 34 million residents in the United States and Canada, representing about 8% and 32% of their respective populations. These inhabitants depend on the region's natural resources for drinking water, commerce, and recreation. Because water level variability can have considerable impacts on communities and infrastructure (e.g., flooding), accurate weather forecasting is critically important for effective management and planning.

## Project Objectives
This project aims to enhance the accuracy of subseasonal-to-annual weather and climate forecasts in the Great Lakes region by using advanced machine learning techniques to correct biases in existing forecast systems. Specifically, we will develop a Convolutional Gaussian Neural Process (ConvGNP) model using the DeepSensor Python package to refine weather forecasting products. DeepSensor can learn complex, geographically and temporally varying covariance structures, making it well-suited for bias correction. It also features built-in uncertainty quantification, allowing for a measure of confidence to be taken into account. As a result of this project, we will produce a ConvGNP bias correction model that will be useful for water level forecasting.

## Objectives
1. **Data Handling:** Gather and preprocess weather forecast and reanalysis datasets for analysis, with assistance from the supervisor.
2. **Model Implementation:** Set up and configure the DeepSensor Python package to develop the Convolutional Gaussian Neural Process (ConvGNP) model.
3. **Training and Validation:** Train the ConvGNP model on historical data, apply it to newer forecast data, and validate the results. (Students can use GPUs on U-M HPC for this task.)
4. **Interpretation:** Examine and characterize the spatial and temporal biases identified by the ConvGNP.
5. **Documentation:** Maintain documentation of methods, processes, and findings for transparency and reproducibility.
6. **Presentation:** Prepare a poster and other materials for the UROP Research Symposium to present the results of the project.

## Student Team
- **Zhichen Yang:** Focus on (specific variables or combinations of variables to be determined), with collaboration on methods and results.
- **Haorui Wang:** Focus on (specific variables or combinations of variables to be determined), with collaboration on methods and results.

## Repository Structure
- `data/`: Dataset files and any supplementary data.
  - `README.md`: Detailed description of the datasets and their features.
- `notebooks/`: Jupyter notebooks/Google Colab notebooks for data analysis and model development.
  - `learning/`: Notebooks illustrating general concepts and techniques.
  - `exploratory/`: Notebooks for exploratory data analysis, model implementation, training, and validation.
- `src/`: Source code for data preprocessing, model development, and analysis scripts.
- `docs/`: Documentation of methodologies, processes, and findings.
- `presentations/`: Materials for the UROP Research Symposium.
- `README.md`: Project overview and getting started guide.
- `LICENSE`: Licensing information.

- ## Issues and Contributions
Feel free to open issues for any questions or contributions. For major changes, please open a pull request first to discuss what you would like to change. (NOTE: This is not necessary if you are just adding a Google Colab notebook to the repository). 

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact
For any further questions, please contact Dani Jones, supervisor.

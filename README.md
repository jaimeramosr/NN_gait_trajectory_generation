# Python Project.

<h1>📊 Neural Network to generate gait kinematic trajectories with Python </h1>
<p>
	This project focuses on creating a Neural Network, preparing input data for its training and testing, and reporting the results. These processes are conducted using Python.
    <p>
Para situarnos, el objetivo es, a partir de los parámetros antropométricos de una persona (longitud de fémur, tibia y anchura de caderas) y unos objetivos de la marcha (longitud y altura de paso), calcular las trayectorias articulares necesarias para que un robot ejecute ese paso.
</p>
<p>
Se cuenta con un algoritmo que hace esto. Sin embargo, para cada conjunto de parámetros de entrada, el algoritmo tarda alrededor de 5 minutos en calcular las trayectorias articulares, no siendo válido para su uso en tiempo real. Por ello, se considera crear una red neuronal para, a partir de datos obtenidas del algoritmo, ser entrenada y utilizada en tiempo real.
</p>
    CAMBIAR IMAGEN
    <p align="center">
  <img src="Images/Sales_Dashboard.png" alt="Sales Dashboard" width="700">
</p>

---
<h2>🛠️ Neural Network design with Python</h2>
<h2>🛠️ Data preparation</h2>
<i>1. Importing & cleaning data </i> <br>
The original dataset (<i>"data_raw.csv"</i>) was reviewed in Excel to understand its structure, identify irrelevant columns, and check for data types.
<p>
The data was composed of one first column containing the cost of an external algorithm´s fitness function when generating the training data. For now, it is not útil. then, this column is deleted, obtaining <i>"data_complete.csv"</i>.
</p>
<p>
    El resto del archivo se compone de 5 columnas con los datos de entrada, en metros que se convierten a centímetros, y 30 columnas que contienen los 15 puntos de inflexión de las trayectorias (15 coordenadas X y 15 coordenadas Y alternas) para luego, haciendo uso de la función "spline", calcular las trayectorias completas. Las columnas impares contienen valores en porcentaje del ciclo de la marcha y las columnas pares las amplitudes en grados.
</p>
<p align="center">
<img src="Images/Import_window.png" alt="SQL Import Window" width="250">
</p>

<i>2. Training, validation, and testing dataset preparation</i> <br>
As previously explained, the input dataset is split into three groups: training (80%), validation (15%), and testing (5%). The training set is used to train the neural network, the validation set monitors performance during training, and the test set is the one used in this assessment process. This step enables the selection of the best-performing trained model.
<p>
    Para ello, de forma aleatoria, se separan las filas en 3 archivos: <i>"data_train.csv", data_valid.csv", y "data_test.csv"</i>.
</p>

---
<h2>📈 NN training</h2>
<i>1. Importing Cleaned Data </i> <br>
<p>
Cleaned data was imported into Power BI as .csv or directly from the MySQL database. 
</p>
<i>2. data Transformation with DAX </i> <br>
<p>
New measures created:<strong>Total Revenue, Total Profit, Total Discounts Given, and Average Order Revenue</strong>.
</p>
<p>
Additional columns: <strong>Month Name, Month Number, Day Name, Day Number</strong> — useful for chronological charts and trend lines.
</p>
<i>3. Dashboard Design </i> <br>
Design principles:

- Focused on executive-level KPIs.
- Key metrics displayed using value cards: Total Revenue, Total Profit, Total Orders, etc.

Visuals include:

- Bar charts (e.g., daily revenue, revenue by category)
- Line charts (e.g., monthly trends)	
- Map charts and scatter plots
- Filters for dynamic analysis (e.g., by country or store)
<p align="center">
<img src="Images/Sales_Dashboard.png" alt="Sales Dashboard" width="700">
</p>

---
<h2>📈 NN testing</h2>

---
<h2>✅ Key Insights from the Sales Dashboard</h2>

- December stands out as the month with highest revenue (~0.52M) — likely influenced by seasonal/holiday shopping patterns.
- Tuesdays and Fridays underperform compared to other days — ideal for scheduling non-sales activities like inventory audits or staff training.
- Discounts correlate with profit — suggesting discounts may be used strategically rather than harming margins.
- Payment method usage is balanced — Cash, Credit Card, and Mobile Payments each contribute ~33%, indicating no dominant preference across users.
- Top revenue categories are Home & Kitchen, Clothing, and Electronics — potential areas for promotion and cross-selling.
- Geographic revenue differences are significant — London, Birmingham, New York and Montreal generate higher profits and/or discounts.
- Nigeria, India, and China show cluster patterns with lower discounts but decent profits — possible candidates for margin optimization.

Further insights can be uncovered using Power BI filters to segment by country, store, or product category.
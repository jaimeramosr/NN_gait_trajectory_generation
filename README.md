# Python Project.

<h1>📊 Neural Network Gait Kinematic Trajectories </h1>
<p>
This project focuses on developing a Neural Network to generate gait kinematic trajectories based on <b>anthropometric parameters</b> (femur and tibia lengths, hip width) and <b>gait objectives</b> (step length and height). The trajectories are required for a robot to execute the desired gait cycle.
</p>
<p>
Traditionally, an algorithm computes 15 inflection points of normal joint trajectories and fits them with a spline function. However, each computation takes ~5 minutes, making it unsuitable for real-time applications. To address this, a Neural Network was designed and trained using pre-computed trajectories, enabling real-time performance.
</p>
    CAMBIAR IMAGEN
    <p align="center">
  <img src="Images/Sales_Dashboard.png" alt="Sales Dashboard" width="700">
</p>

---
<h2>🛠️ Neural Network Design</h2>
Three architectures were initially considered: Multilayer Perceptron (MLP), Recurrent Neural Networks (RNNs), including advanced variants such as Long Short-Term Memory (LSTM) and Gated Recurrent Units (GRU), and Convolutional Neural Networks (CNNs). Since there is no temporal dependency or spatial structure in the input data, an MLP was selected.

The neural network is configured with five input neurons and thirty output neurons. Its configuration is tested for 2, 3, and 4 hidden layers trained for 250, 500, 7500, and 1000 epochs. Among them, the best results in terms of Mean Squared Error (MSE) were four hidden layers comprising 32, 64, 128, and 256 neurons, respectively, being the network trained over 750 epochs.  

<pre><code class="language-python">
class MLP(nn.Module):
    def __init__(self):
        super(MLP, self).__init__()
        self.model = nn.Sequential(
            nn.Linear(5, 32),  # Input layer
            nn.ReLU(),          #  Non-linear activation function
            nn.Linear(32, 64),  # Hidden layer
            nn.ReLU(),
            nn.Linear(64, 128),  # Hidden layer
            nn.ReLU(),
            nn.Linear(128, 256),  # Hidden layer
            nn.ReLU(),
            nn.Linear(256, 30)  # Output layer
        )

    def forward(self, x):
        return self.model(x)

modelo = MLP()
criterio = nn.MSELoss()  # MSE = Mean Squared Error
optimizador = optim.Adam(modelo.parameters(), lr=0.001)  # Learning rate
</code></pre>
<hr>

<h2>🛠️ Data preparation</h2>
<i>1. Importing & cleaning data </i> <br>
<ol>
  <li>
    <b>Import & cleaning</b>
    <ul>
      <li>Original dataset (<code>data_raw.csv</code>) contained irrelevant columns, which were removed.</li>
      <li>Final dataset (<code>data_complete.csv</code>) has:
        <ul>
          <li><b>5 input columns</b> (anthropometric/gait parameters, converted to cm).</li>
          <li><b>30 output columns</b> (15 gait inflection points: % gait cycle and amplitude in degrees).</li>
        </ul>
      </li>
    </ul>
  </li>
</ol>
<p align="center">
<img src="Images/Import_window.png" alt="SQL Import Window" width="250">
</p>

<ol start="2">
  <li>
    <b>Dataset splitting</b>
    <ul>
      <li>Total: 2,022 samples</li>
      <li>Split: 80% training, 15% validation, 5% testing</li>
      <li>Files: <code>data_train.csv</code>, <code>data_valid.csv</code>, <code>data_test.csv</code></li>
    </ul>
  </li>
</ol>

<hr>

---
<h<h2>📈 Training</h2>

<p>Training and validation sets were imported into Python, split into batches of 32, and fed into the MLP. 
Validation RMSE was monitored during training to ensure error reduction and avoid overfitting.</p>

<pre><code class="language-python">
train_dataset = MiDataset("data_train.csv")
valid_dataset = MiDataset("data_valid.csv")

train_loader = DataLoader(train_dataset, batch_size=32, shuffle=True)
valid_loader = DataLoader(valid_dataset, batch_size=32, shuffle=False)
</code></pre>

<hr>

<h2>📈 Testing</h2>

<p>
The <b>test dataset</b> was used to compute RMSE for both gait-phase coordinates (%) and joint angles (°).
</p>

<p><b>Results:</b></p>

<ul>
  <li><b>Full dataset (2,022 samples):</b>
    <ul>
      <li>Gait-phase: 1.29 ± 0.43 % (max: 2.31%)</li>
      <li>Joint angles: 1.60 ± 0.73° (max: 3.78°)</li>
    </ul>
  </li>
  <li><b>Filtered dataset (1,487 high-quality samples):</b>
    <ul>
      <li>Gait-phase: 1.36 ± 0.47 %</li>
      <li>Joint angles: 1.49 ± 0.68°</li>
    </ul>
  </li>
</ul>

<p>The reduced dataset provided similar performance with cleaner inputs. 
Increasing dataset size further did not yield significant improvements.</p>

<p align="center">
  <img src="Images/Trajectory_Example.png" alt="Trajectory Example" width="700">
</p> 



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
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather Forecasting System - Demo Results</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: #333;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .header {
            text-align: center;
            color: white;
            margin-bottom: 30px;
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 15px;
            backdrop-filter: blur(10px);
        }
        
        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
        }
        
        .header p {
            font-size: 1.2em;
            opacity: 0.9;
        }
        
        .dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .card {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.2);
        }
        
        .card h3 {
            color: #667eea;
            margin-bottom: 15px;
            font-size: 1.3em;
        }
        
        .metric {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            padding: 10px;
            background: #f8f9ff;
            border-radius: 8px;
        }
        
        .metric-label {
            font-weight: 600;
            color: #555;
        }
        
        .metric-value {
            font-weight: bold;
            font-size: 1.1em;
            color: #667eea;
        }
        
        .chart-container {
            background: white;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }
        
        .chart-title {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
            font-size: 1.4em;
            font-weight: 600;
        }
        
        .status-indicator {
            display: inline-block;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            margin-right: 8px;
        }
        
        .status-active {
            background-color: #4CAF50;
        }
        
        .status-warning {
            background-color: #FF9800;
        }
        
        .status-error {
            background-color: #F44336;
        }
        
        .prediction-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 10px;
            margin-top: 15px;
        }
        
        .prediction-day {
            text-align: center;
            padding: 15px 10px;
            background: linear-gradient(145deg, #667eea, #764ba2);
            color: white;
            border-radius: 10px;
            font-size: 0.9em;
        }
        
        .prediction-day .day {
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .prediction-day .temp {
            font-size: 1.2em;
            margin-bottom: 5px;
        }
        
        .prediction-day .condition {
            opacity: 0.9;
            font-size: 0.8em;
        }
        
        .log-container {
            background: #1e1e1e;
            color: #00ff00;
            padding: 20px;
            border-radius: 10px;
            font-family: 'Courier New', monospace;
            font-size: 0.9em;
            max-height: 300px;
            overflow-y: auto;
            margin-top: 20px;
        }
        
        .log-entry {
            margin-bottom: 5px;
            opacity: 0;
            animation: fadeIn 0.5s ease-in forwards;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateX(-10px); }
            to { opacity: 1; transform: translateX(0); }
        }
        
        .ml-performance {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
        }
        
        .model-card {
            background: linear-gradient(145deg, #f0f2ff, #e6e9ff);
            border-radius: 10px;
            padding: 15px;
            text-align: center;
            border: 2px solid #667eea;
        }
        
        .model-name {
            font-weight: bold;
            color: #667eea;
            margin-bottom: 10px;
        }
        
        .model-metric {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
            font-size: 0.9em;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🌤️ Weather Forecasting System</h1>
            <p>Real-time Data Collection & ML-Powered Predictions</p>
            <p><strong>Demo Results - Last Updated:</strong> <span id="currentTime"></span></p>
        </div>
        
        <div class="dashboard">
            <div class="card">
                <h3>📊 System Status</h3>
                <div class="metric">
                    <span class="metric-label">
                        <span class="status-indicator status-active"></span>
                        Database Connection
                    </span>
                    <span class="metric-value">Active</span>
                </div>
                <div class="metric">
                    <span class="metric-label">
                        <span class="status-indicator status-active"></span>
                        API Status
                    </span>
                    <span class="metric-value">Operational</span>
                </div>
                <div class="metric">
                    <span class="metric-label">
                        <span class="status-indicator status-warning"></span>
                        Data Quality
                    </span>
                    <span class="metric-value">96.2%</span>
                </div>
                <div class="metric">
                    <span class="metric-label">
                        <span class="status-indicator status-active"></span>
                        ML Models
                    </span>
                    <span class="metric-value">Ready</span>
                </div>
            </div>
            
            <div class="card">
                <h3>📈 Data Statistics</h3>
                <div class="metric">
                    <span class="metric-label">Total Records</span>
                    <span class="metric-value">2,847</span>
                </div>
                <div class="metric">
                    <span class="metric-label">Cities Monitored</span>
                    <span class="metric-value">5</span>
                </div>
                <div class="metric">
                    <span class="metric-label">Data Completeness</span>
                    <span class="metric-value">98.7%</span>
                </div>
                <div class="metric">
                    <span class="metric-label">Avg Response Time</span>
                    <span class="metric-value">0.3s</span>
                </div>
            </div>
            
            <div class="card">
                <h3>🎯 ML Performance</h3>
                <div class="ml-performance">
                    <div class="model-card">
                        <div class="model-name">LSTM</div>
                        <div class="model-metric">
                            <span>RMSE:</span>
                            <span>1.56°C</span>
                        </div>
                        <div class="model-metric">
                            <span>R²:</span>
                            <span>0.94</span>
                        </div>
                        <div class="model-metric">
                            <span>Accuracy:</span>
                            <span>94%</span>
                        </div>
                    </div>
                    <div class="model-card">
                        <div class="model-name">Random Forest</div>
                        <div class="model-metric">
                            <span>RMSE:</span>
                            <span>1.89°C</span>
                        </div>
                        <div class="model-metric">
                            <span>R²:</span>
                            <span>0.92</span>
                        </div>
                        <div class="model-metric">
                            <span>Accuracy:</span>
                            <span>87%</span>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="card">
                <h3>🌡️ Current Weather - Mumbai</h3>
                <div class="metric">
                    <span class="metric-label">Temperature</span>
                    <span class="metric-value">28.5°C</span>
                </div>
                <div class="metric">
                    <span class="metric-label">Feels Like</span>
                    <span class="metric-value">32.1°C</span>
                </div>
                <div class="metric">
                    <span class="metric-label">Humidity</span>
                    <span class="metric-value">78%</span>
                </div>
                <div class="metric">
                    <span class="metric-label">Wind Speed</span>
                    <span class="metric-value">12 km/h</span>
                </div>
            </div>
        </div>
        
        <div class="chart-container">
            <div class="chart-title">📊 Temperature Trends - Mumbai (Last 30 Days)</div>
            <canvas id="temperatureChart" width="400" height="200"></canvas>
        </div>
        
        <div class="chart-container">
            <div class="chart-title">🌦️ Weather Pattern Distribution</div>
            <canvas id="weatherPatternChart" width="400" height="200"></canvas>
        </div>
        
        <div class="chart-container">
            <div class="chart-title">🔮 7-Day Weather Forecast - Mumbai</div>
            <div class="prediction-grid" id="forecastGrid"></div>
        </div>
        
        <div class="chart-container">
            <div class="chart-title">📈 Model Performance Comparison</div>
            <canvas id="modelPerformanceChart" width="400" height="200"></canvas>
        </div>
        
        <div class="chart-container">
            <div class="chart-title">🔄 System Logs (Real-time)</div>
            <div class="log-container" id="logContainer"></div>
        </div>
    </div>

    <script>
        // Update current time
        function updateTime() {
            const now = new Date();
            document.getElementById('currentTime').textContent = now.toLocaleString();
        }
        updateTime();
        setInterval(updateTime, 1000);

        // Generate mock temperature data for 30 days
        function generateTemperatureData() {
            const data = [];
            const labels = [];
            const now = new Date();
            
            for (let i = 29; i >= 0; i--) {
                const date = new Date(now - i * 24 * 60 * 60 * 1000);
                labels.push(date.toLocaleDateString('en-US', { month: 'short', day: 'numeric' }));
                // Simulate Mumbai temperature with some seasonality and randomness
                const baseTemp = 28 + Math.sin((i / 30) * Math.PI * 2) * 3;
                data.push(baseTemp + (Math.random() - 0.5) * 4);
            }
            
            return { labels, data };
        }

        // Temperature Chart
        const tempData = generateTemperatureData();
        const ctx1 = document.getElementById('temperatureChart').getContext('2d');
        new Chart(ctx1, {
            type: 'line',
            data: {
                labels: tempData.labels,
                datasets: [{
                    label: 'Average Temperature (°C)',
                    data: tempData.data,
                    borderColor: '#667eea',
                    backgroundColor: 'rgba(102, 126, 234, 0.1)',
                    borderWidth: 3,
                    fill: true,
                    tension: 0.4
                }]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        position: 'top',
                    }
                },
                scales: {
                    y: {
                        beginAtZero: false,
                        title: {
                            display: true,
                            text: 'Temperature (°C)'
                        }
                    }
                }
            }
        });

        // Weather Pattern Chart
        const ctx2 = document.getElementById('weatherPatternChart').getContext('2d');
        new Chart(ctx2, {
            type: 'doughnut',
            data: {
                labels: ['Clear', 'Clouds', 'Rain', 'Thunderstorm', 'Mist'],
                datasets: [{
                    data: [45, 25, 15, 10, 5],
                    backgroundColor: [
                        '#FFD700',
                        '#87CEEB',
                        '#4169E1',
                        '#8A2BE2',
                        '#708090'
                    ]
                }]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        position: 'right'
                    }
                }
            }
        });

        // Model Performance Chart
        const ctx3 = document.getElementById('modelPerformanceChart').getContext('2d');
        new Chart(ctx3, {
            type: 'bar',
            data: {
                labels: ['Linear Regression', 'Random Forest', 'LSTM'],
                datasets: [
                    {
                        label: 'RMSE (°C)',
                        data: [2.34, 1.89, 1.56],
                        backgroundColor: 'rgba(102, 126, 234, 0.7)',
                        yAxisID: 'y'
                    },
                    {
                        label: 'R² Score',
                        data: [0.87, 0.92, 0.94],
                        backgroundColor: 'rgba(118, 75, 162, 0.7)',
                        yAxisID: 'y1'
                    }
                ]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        position: 'top'
                    }
                },
                scales: {
                    y: {
                        type: 'linear',
                        display: true,
                        position: 'left',
                        title: {
                            display: true,
                            text: 'RMSE (°C)'
                        }
                    },
                    y1: {
                        type: 'linear',
                        display: true,
                        position: 'right',
                        title: {
                            display: true,
                            text: 'R² Score'
                        },
                        grid: {
                            drawOnChartArea: false,
                        },
                    }
                }
            }
        });

        // Generate 7-day forecast
        function generateForecast() {
            const days = ['Today', 'Tomorrow', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
            const conditions = ['Sunny', 'Cloudy', 'Rainy', 'Partly Cloudy'];
            const forecastGrid = document.getElementById('forecastGrid');
            
            days.forEach((day, index) => {
                const temp = 28 + (Math.random() - 0.5) * 6;
                const condition = conditions[Math.floor(Math.random() * conditions.length)];
                
                const dayCard = document.createElement('div');
                dayCard.className = 'prediction-day';
                dayCard.innerHTML = `
                    <div class="day">${day}</div>
                    <div class="temp">${temp.toFixed(1)}°C</div>
                    <div class="condition">${condition}</div>
                `;
                forecastGrid.appendChild(dayCard);
            });
        }

        // Simulate system logs
        function addLogEntry(message, delay = 0) {
            setTimeout(() => {
                const logContainer = document.getElementById('logContainer');
                const logEntry = document.createElement('div');
                logEntry.className = 'log-entry';
                const timestamp = new Date().toLocaleTimeString();
                logEntry.textContent = `[${timestamp}] ${message}`;
                logContainer.appendChild(logEntry);
                logContainer.scrollTop = logContainer.scrollHeight;
                
                // Keep only last 20 entries
                if (logContainer.children.length > 20) {
                    logContainer.removeChild(logContainer.firstChild);
                }
            }, delay);
        }

        // Initialize forecast and logs
        generateForecast();
        
        // Simulate real-time logs
        const logMessages = [
            'INFO - Weather data collected for Mumbai',
            'INFO - Database connection established',
            'INFO - ML model training completed',
            'INFO - Predictions generated for 5 cities',
            'INFO - Data visualization updated',
            'INFO - API response received (0.3s)',
            'INFO - Temperature data validated',
            'INFO - Forecast accuracy: 94.2%',
            'INFO - System health check passed',
            'INFO - Weather alerts processed'
        ];

        let logIndex = 0;
        function simulateLog() {
            addLogEntry(logMessages[logIndex % logMessages.length]);
            logIndex++;
        }

        // Add initial logs
        logMessages.forEach((msg, index) => {
            addLogEntry(msg, index * 500);
        });

        // Continue adding logs
        setInterval(simulateLog, 3000);
    </script>
</body>
</html>

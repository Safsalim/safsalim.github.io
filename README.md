# safsalim.github.io
data cme gaps
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Analysis of CME Bitcoin Futures Weekend Gaps</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #003F5C; 
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 400px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 350px;
        }
        .wide-chart-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            height: 400px;
            max-height: 450px;
        }
        .stat-card {
            background-color: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.2);
            backdrop-filter: blur(10px);
        }
    </style>
</head>
<body class="text-white">
    
    <div class="container mx-auto p-4 md:p-8">

        <header class="text-center mb-12">
            <h1 class="text-4xl md:text-6xl font-black text-white uppercase tracking-wider">Analysis of CME Bitcoin Futures Weekend Gaps</h1>
            <p class="mt-4 text-lg md:text-xl text-gray-300 max-w-3xl mx-auto">An insight into market behavior, comparing historical data against the accelerated trends of 2025.</p>
        </header>

        <main class="grid grid-cols-1 lg:grid-cols-2 gap-8">

            <div class="lg:col-span-2 p-8 rounded-2xl stat-card text-center">
                <h2 class="text-2xl font-bold text-white mb-4">Total Gaps Identified</h2>
                <div class="flex flex-col md:flex-row justify-around items-center">
                    <div class="m-4">
                        <p class="text-7xl font-black text-[#FFA600]">96</p>
                        <p class="text-lg text-gray-300">Across Full Dataset</p>
                    </div>
                    <div class="m-4">
                        <p class="text-7xl font-black text-[#EF5675]">34</p>
                        <p class="text-lg text-gray-300">In 2025 Alone</p>
                    </div>
                </div>
                <p class="mt-4 text-gray-400">The analysis covers a substantial historical dataset, with a significant number of recent occurrences in 2025 providing a basis for trend analysis.</p>
            </div>
            
            <div class="p-6 rounded-2xl stat-card flex flex-col items-center">
                <h3 class="text-xl font-bold text-center mb-4">Percentage of Gaps Closed</h3>
                <p class="text-center text-gray-400 mb-4 flex-grow">A comparison of the rate at which weekend gaps eventually fill, highlighting the increased certainty in 2025.</p>
                <div class="chart-container">
                    <canvas id="percentageClosedChart"></canvas>
                </div>
            </div>

            <div class="p-6 rounded-2xl stat-card flex flex-col items-center">
                <h3 class="text-xl font-bold text-center mb-4">Gaps Closed Within 1 Week</h3>
                 <p class="text-center text-gray-400 mb-4 flex-grow">This metric shows the proportion of gaps that fill quickly, demonstrating a significant increase in short-term closure speed in 2025.</p>
                <div class="chart-container">
                    <canvas id="closedWithinWeekChart"></canvas>
                </div>
            </div>

            <div class="lg:col-span-2 p-6 rounded-2xl stat-card flex flex-col items-center">
                <h3 class="text-xl font-bold text-center mb-4">Average Time to Close</h3>
                <p class="text-center text-gray-400 mb-4 flex-grow">The average duration for a gap to fill has drastically decreased, pointing to a more efficient market in 2025.</p>
                <div class="chart-container w-full max-w-lg">
                    <canvas id="avgTimeToCloseChart"></canvas>
                </div>
            </div>
            
            <div class="lg:col-span-2 p-6 rounded-2xl stat-card">
                <h3 class="text-xl font-bold text-center mb-2">Distribution of Gap Closing Times (Full Dataset)</h3>
                <p class="text-center text-gray-400 mb-4 max-w-3xl mx-auto">A significant portion of gaps (41%) are filled within the first 24 hours, and over 70% close within the first three days, showing a front-loaded closure pattern.</p>
                <div class="wide-chart-container">
                    <canvas id="fullDatasetDistributionChart"></canvas>
                </div>
            </div>
            
            <div class="lg:col-span-2 p-6 rounded-2xl stat-card">
                <h3 class="text-xl font-bold text-center mb-2">Distribution of Gap Closing Times (2025 Only)</h3>
                <p class="text-center text-gray-400 mb-4 max-w-3xl mx-auto">The trend of rapid gap-fills is even more pronounced in 2025, with nearly 70% closing in the first 24 hours and no gaps remaining open for more than 5 days.</p>
                <div class="wide-chart-container">
                    <canvas id="twentyTwentyFiveDistributionChart"></canvas>
                </div>
            </div>

        </main>

        <footer class="text-center mt-12 py-6 border-t border-gray-700">
            <p class="text-gray-400">Key Takeaway: The 2025 data indicates a clear market trend towards filling weekend gaps with greater speed and higher frequency than the historical average.</p>
        </footer>

    </div>

    <script>
        const tooltipTitleCallback = {
            plugins: {
                tooltip: {
                    callbacks: {
                        title: function(tooltipItems) {
                            const item = tooltipItems[0];
                            let label = item.chart.data.labels[item.dataIndex];
                            if (Array.isArray(label)) {
                              return label.join(' ');
                            } else {
                              return label;
                            }
                        }
                    }
                },
                legend: {
                    labels: {
                        color: '#D1D5DB' 
                    }
                }
            }
        };

        new Chart(document.getElementById('percentageClosedChart'), {
            type: 'doughnut',
            data: {
                labels: ['Full Dataset (95.83%)', '2025 (97.06%)'],
                datasets: [{
                    data: [95.83, 97.06],
                    backgroundColor: ['#FFA600', '#EF5675'],
                    borderColor: '#003F5C',
                    borderWidth: 4,
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                ...tooltipTitleCallback,
                plugins: {
                    ...tooltipTitleCallback.plugins,
                    title: {
                        display: false
                    }
                },
                cutout: '60%',
            }
        });

        new Chart(document.getElementById('closedWithinWeekChart'), {
            type: 'doughnut',
            data: {
                labels: ['Full Dataset (84.38%)', '2025 (94.12%)'],
                datasets: [{
                    data: [84.38, 94.12],
                    backgroundColor: ['#FFA600', '#EF5675'],
                    borderColor: '#003F5C',
                    borderWidth: 4,
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                ...tooltipTitleCallback,
                plugins: {
                    ...tooltipTitleCallback.plugins,
                    title: {
                        display: false
                    }
                },
                cutout: '60%',
            }
        });

        new Chart(document.getElementById('avgTimeToCloseChart'), {
            type: 'bar',
            data: {
                labels: ['Full Dataset', '2025'],
                datasets: [{
                    label: 'Average Days to Close',
                    data: [4.58, 1.43],
                    backgroundColor: ['#FFA600', '#EF5675'],
                    borderRadius: 6,
                    barThickness: 50,
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                ...tooltipTitleCallback,
                scales: {
                    y: {
                        beginAtZero: true,
                        grid: { color: 'rgba(255, 255, 255, 0.1)' },
                        ticks: { color: '#D1D5DB' },
                        title: { display: true, text: 'Days', color: '#D1D5DB' }
                    },
                    x: {
                       grid: { display: false },
                       ticks: { color: '#D1D5DB' }
                    }
                },
                plugins: { ...tooltipTitleCallback.plugins, legend: { display: false } }
            }
        });

        new Chart(document.getElementById('fullDatasetDistributionChart'), {
            type: 'bar',
            data: {
                labels: ['0-1 Days', '1-2 Days', '2-3 Days', '3-4 Days', '4-5 Days', '5-6 Days', '6-7 Days', '7+ Days'],
                datasets: [{
                    label: 'Number of Gaps Closed',
                    data: [38, 17, 10, 6, 6, 2, 3, 10],
                    backgroundColor: '#FFA600',
                    borderRadius: 6,
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                ...tooltipTitleCallback,
                scales: {
                    y: {
                        beginAtZero: true,
                        grid: { color: 'rgba(255, 255, 255, 0.1)' },
                        ticks: { color: '#D1D5DB' },
                        title: { display: true, text: 'Number of Gaps', color: '#D1D5DB' }
                    },
                    x: {
                       grid: { display: false },
                       ticks: { color: '#D1D5DB' },
                       title: { display: true, text: 'Time to Close', color: '#D1D5DB' }
                    }
                },
                plugins: { ...tooltipTitleCallback.plugins, legend: { display: false } }
            }
        });
        
        new Chart(document.getElementById('twentyTwentyFiveDistributionChart'), {
            type: 'bar',
            data: {
                labels: ['0-1 Days', '1-2 Days', '2-3 Days', '3-4 Days', '4-5 Days', '5+ Days'],
                datasets: [{
                    label: 'Number of Gaps Closed',
                    data: [23, 5, 2, 2, 1, 0],
                    backgroundColor: '#EF5675',
                    borderRadius: 6,
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                ...tooltipTitleCallback,
                scales: {
                    y: {
                        beginAtZero: true,
                        grid: { color: 'rgba(255, 255, 255, 0.1)' },
                        ticks: { color: '#D1D5DB' },
                        title: { display: true, text: 'Number of Gaps', color: '#D1D5DB' }
                    },
                    x: {
                       grid: { display: false },
                       ticks: { color: '#D1D5DB' },
                       title: { display: true, text: 'Time to Close', color: '#D1D5DB' }
                    }
                },
                plugins: { ...tooltipTitleCallback.plugins, legend: { display: false } }
            }
        });
    </script>
</body>
</html>

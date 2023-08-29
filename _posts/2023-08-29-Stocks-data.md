---
toc: true
comments: true
layout: post
title: Stocks data
courses: { calendar: {week: 2} }
type: hacks
---


<html>
<head>
    <title>Stock Data Table</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <h1>Stock Data Table</h1>
    <table id="stockTable">
        <thead>
            <tr>
                <th>Date</th>
                <th>Symbol</th>
                <th>Open</th>
                <th>High</th>
                <th>Low</th>
                <th>Close</th>
                <th>Volume</th>
            </tr>
        </thead>
        <tbody id="stockTableBody">
            <!-- Stock data will be populated here -->
        </tbody>
    </table>
    <canvas id="stockChart" width="400" height="200"></canvas>


    <script>
        const apiKey = 'I7RX9CDHLG7AROX8';
        const symbol = 'AAPL'; // Replace with the desired stock symbol

        const apiUrl = `https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=${symbol}&apikey=${apiKey}`;

        fetch(apiUrl)
            .then(response => response.json())
            .then(data => {
                const stockData = data['Time Series (Daily)'];

                const stockTableBody = document.getElementById('stockTableBody');

                for (const date in stockData) {
                    if (stockData.hasOwnProperty(date)) {
                        const rowData = stockData[date];
                        const row = document.createElement('tr');

                        const dateCell = document.createElement('td');
                        dateCell.textContent = date;
                        row.appendChild(dateCell);

                        const symbolCell = document.createElement('td');
                        symbolCell.textContent = symbol;
                        row.appendChild(symbolCell);

                        const openCell = document.createElement('td');
                        openCell.textContent = rowData['1. open'];
                        row.appendChild(openCell);

                        const highCell = document.createElement('td');
                        highCell.textContent = rowData['2. high'];
                        row.appendChild(highCell);

                        const lowCell = document.createElement('td');
                        lowCell.textContent = rowData['3. low'];
                        row.appendChild(lowCell);

                        const closeCell = document.createElement('td');
                        closeCell.textContent = rowData['4. close'];
                        row.appendChild(closeCell);

                        const volumeCell = document.createElement('td');
                        volumeCell.textContent = rowData['5. volume'];
                        row.appendChild(volumeCell);

                        stockTableBody.appendChild(row);
                    }
                }
            })
            .catch(error => {
                console.error('Error fetching stock data:', error);
            });

            

    // Graph script

    const labels = []; // Array to hold the dates
    const openPrices = []; // Array to hold the opening prices

    for (const date in stockData) {
        if (stockData.hasOwnProperty(date)) {
            labels.push(date);
            openPrices.push(parseFloat(stockData[date]['1. open']));
        }
    }

    const ctx = document.getElementById('stockChart').getContext('2d');
    const stockChart = new Chart(ctx, {
        type: 'line',
        data: {
            labels: labels,
            datasets: [{
                label: 'Opening Prices',
                data: openPrices,
                borderColor: 'blue',
                fill: false,
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                x: {
                    type: 'time',
                    time: {
                        unit: 'day',
                        displayFormats: {
                            day: 'MMM D'
                        },
                    },
                    title: {
                        display: true,
                        text: 'Date'
                    }
                },
                y: {
                    title: {
                        display: true,
                        text: 'Open Price'
                    }
                }
            }
        }
    });
    </script>
</body>
</html>



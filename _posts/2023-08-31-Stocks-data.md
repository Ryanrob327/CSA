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
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>
<body>
    <a>*Data may take a moment to load</a>
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

    <h1>Stock Chart</h1>
    <div id="chart"></div>

    <script>
        const apiKey = 'I7RX9CDHLG7AROX8';
        const symbol = 'AAPL'; 

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

        fetch(apiUrl)
            .then(response => response.json())
            .then(data => {
                const dates = Object.keys(data['Time Series (Daily)']).reverse();
                const prices = dates.map(date => parseFloat(data['Time Series (Daily)'][date]['4. close']));
                
                const trace = {
                    x: dates,
                    y: prices,
                    type: 'scatter',
                    mode: 'lines',
                    name: 'AAPL Stock Price'
                };
                
                const layout = {
                    title: 'AAPL Stock Price',
                    xaxis: {
                        title: 'Date'
                    },
                    yaxis: {
                        title: 'Price (USD)'
                    }
                };
                
                const config = {
                    responsive: true
                };
                
                Plotly.newPlot('chart', [trace], layout, config);
            })
            .catch(error => console.error('Error fetching data:', error));
    </script>
</body>
</html>

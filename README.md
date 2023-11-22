# Currency-converter-app-using-javascript
HTML,CSS and JS is added in the same file of HTML.
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
           background-color: blueviolet;
            font-family: Arial, sans-serif;
            text-align: center;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-image: url("/home/smith/Downloads/picture.png");
        
        }

        #container{
            width: 60vh;
           background-color: white;
           border-radius: 5vh;
        }

        #converter {
            max-width: 400px;
            margin: 50px auto;
        }

        input {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            box-sizing: border-box;
        }
        h1{
            font-family: cursive;
            
        }

        #btn {
            width: 40%;
            padding: 10px;
            background-color: orange;
            color: white;
            font-size: larger;
            border: none;
            cursor: pointer;
            margin-top: 30px;
            border-radius: 20px;
        }

        #result {
            font-size: 2em;
            margin-top: 10px;
            color: dimgray;
            font-style: italic;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: gold;
        }

        #arrow {
            position: relative;
            width: 0;
            height: 0;
            border-left: 20px solid transparent;
            border-right: 20px solid transparent;
            border-top: 20px solid #333;
            margin-left: 45%;
        }

        #fromCurrency {
            width: 100%;
            margin-bottom: 10px;
        }

        #toCurrency {
            width: 100%;
            margin-top: 10px;
        }
    </style>
    <title>Currency Converter</title>
</head>

<body>
    <div id = "container">
    <div id="converter">
        <h1>Currency Converter</h1>
        <input type="number" id="amount" placeholder="Enter amount">
        <select id="fromCurrency">
            <!-- Add more currencies as needed -->
        </select>
        <div id="arrow"></div>
        <select id="toCurrency" style="width: 100%;">
            <!-- Add more currencies as needed -->
        </select>
        <button id="btn">Convert</button>
        <div id="result"></div>
    </div>

    <script>
        const btn = document.querySelector("#btn");
        const fromCurrencySelect = document.querySelector("#fromCurrency");
        const toCurrencySelect = document.querySelector("#toCurrency");
        const result = document.querySelector("#result");

        async function fetchApi() {
            try {
                const response = await fetch("https://v6.exchangerate-api.com/v6/821f828c1ad40818c29eb966/latest/USD");
                if (!response.ok) {
                    throw new Error(`Error occurred: ${response.status}`);
                }
                const data = await response.json();
                return data;
            } catch (error) {
                console.error('Error:', error);
            }
        }

        async function populateCurrencySelects() {
            const data = await fetchApi();
            const currencies = Object.keys(data.conversion_rates);
            currencies.forEach(currency => {
                const option = document.createElement('option');
                option.value = currency;
                option.textContent = currency;
                fromCurrencySelect.appendChild(option);

                const toCurrencyOption = option.cloneNode(true);
                toCurrencySelect.appendChild(toCurrencyOption);
            });
        }

        populateCurrencySelects();

        btn.addEventListener('click', async () => {
            const fromCurrency = fromCurrencySelect.value;
            const toCurrency = toCurrencySelect.value;
            const amount = document.querySelector('#amount').value;

            if (!amount) {
                result.textContent = 'Please enter a valid amount.';
                return;
            }

            const data = await fetchApi();
            const exchangeRate = data.conversion_rates[toCurrency];

            if (exchangeRate) {
                const convertedAmount = amount * exchangeRate;
                result.innerHTML = `${amount} ${fromCurrency} = ${convertedAmount.toFixed(2)} ${toCurrency}`;
            }
        });
    </script>
    </div>

</body>

</html>

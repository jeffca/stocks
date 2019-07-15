<script>
    import axios from 'axios';

    let latest_stock_prices = [];

    let margins = [];

    axios({
        method: "GET",
        url: 'https://api.steinhq.com/v1/storages/5d2cb7bb490adc53ef5c2b06/Sheet1',                
        timeout: 9000,
    })
    .then(function (response) {
        for (var i = 0; i < response.data.length; i++) {
            latest_stock_prices.push(response.data[i]);            
        }
        console.log(latest_stock_prices);
        latest_stock_prices = latest_stock_prices;
        axios({
            method: "GET",
            url: "https://api.steinhq.com/v1/storages/5d2cb7bb490adc53ef5c2b06/Buys",
            timeout: 9000,
        })
        .then(function (response) {
            console.log(response.data);
            for (var i = 0; i < response.data.length; i++) {
                for (var j = 0; j < latest_stock_prices.length; j++) {
                    if (response.data[i]['Stock'] == latest_stock_prices[j]['Stock']) {
                        margins.push({
                            'Buy_Price': response.data[i]['Price'],
                            'Buy_Quantity': response.data[i]['Quantity'],
                            'Buy_Date': response.data[i]['Date'],
                            'Today_Price': latest_stock_prices[j]['Price']
                        });
                    }
                }
            }
            console.log(margins);
        });
    });



</script>
<!-- 
{#each latest_stock_prices as result}

    <div>{result.Stock}: ${result.Price}</div>

{/each}     -->

{#each margins as m}

    <h1>{m.Buy_Price}</h1>
    <div>{m.Buy_Quantity}</div>
    <div>{m.Buy_Date}</div>
    <div>{m.Today_Price}</div>

{/each}
To add a new chart please check these resource first of all
- How to use ChartJs https://www.chartjs.org/docs/latest/getting-started/

Add the canvas to the html

```html
  <div class = "row">
    <div class="col-6">
      <div class=" hero orderbook">
        <h3 style="text-align:center">Order Book 1 </h3>
        <div class="chart-container">
          <table class='trade-table'>
              <tbody id="orderbook-table-1">
          </table>
        </div>
      </div>
    </div> 
    <div class="col-6">
      <div class=" hero orderbook">
        <h3 style="text-align:center">Order Book 2</h3>
        <div class="chart-container">
          <table class='trade-table'>
              <tbody id="orderbook-table-2">
          </table>
        </div>
      </div>
    </div>
  </div>
  <div class="row">
    <div class="col-12 hero orderbook">
      <h3 style="text-align:center">Order Book 3</h3>
      <div class="chart-container">
        <table class='trade-table'>
            <tbody id="orderbook-table-3">
        </table>
      </div>
    </div>
  </div>
```

```js
  const orderbook_1 = document.getElementById("orderbook-table-1");
  const orderbook_2 = document.getElementById("orderbook-table-2");
  const orderbook_3 = document.getElementById("orderbook-table-3");
```


### Websocket with the backend

```js
  socket.on("updateOrderbookData", function (msg) {
    apiData = [
      {
        bid_size: msg.value.bid_size,
        bid_price: msg.value.bid_price,
        ask_price: msg.value.ask_price,
        ask_size: msg.value.ask_size,
      },
    ];
    renderDataInTheTable(orderbook_1, apiData);
    renderDataInTheTable(orderbook_2, apiData);
    renderDataInTheTable(orderbook_3, apiData);
  });
```

### Grid orderbook

```js
  function renderDataInTheTable(chartObject, datas) {
    if (chartObject.children.length > 4) {
      chartObject.getElementsByTagName("tr")[1].remove();
    }
    datas.forEach((todo) => {
      let newRow = document.createElement("tr");
      newRow.className = "value-detail";
      Object.keys(todo).forEach((key) => {
        let cell = document.createElement("td");
        if (key == "bid_price" || key == "bid_size") {
          cell.className = "bid-detail";
        } else {
          cell.className = "ask-detail";
        }
        cell.innerText = todo[key];
        newRow.appendChild(cell);
      });
      chartObject.appendChild(newRow);
    });
  }
```

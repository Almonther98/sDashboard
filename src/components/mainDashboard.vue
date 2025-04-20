<template>
  <div class="max-w-5xl mx-auto p-5 bg-teal-100 rounded-lg shadow-md">
    <h1 class="text-3xl font-bold text-center text-slate-900 mb-8">
      Tech Stock Dashboard
    </h1>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
      <div
        v-for="stock in stocks"
        :key="stock.symbol"
        class="bg-yellow-100 rounded-lg shadow-sm p-5 transition-transform duration-200 hover:-translate-y-1"
        :class="{
          'border-l-4 border-green-500': stock.change > 0,
          'border-l-4 border-red-500': stock.change < 0,
        }"
      >
        <div class="mb-4">
          <h2 class="text-lg font-semibold text-slate-900 m-0">
            {{ stock.name }}
          </h2>
          <div class="text-sm text-slate-900">{{ stock.symbol }}</div>
        </div>

        <div class="text-2xl text-slate-900 font-bold mb-3">
          ${{ stock.price.toFixed(2) }}
        </div>

        <div :class="stock.change > 0 ? 'text-green-500' : 'text-red-500'">
          <span>{{ stock.change > 0 ? "▲" : "▼" }}</span>
          <span
            >${{ Math.abs(stock.change).toFixed(2) }} ({{
              stock.changePercent.toFixed(2)
            }}%)</span
          >
        </div>
      </div>
    </div>

    <div class="text-center text-slate-900 text-m mb-5">
      Last updated: {{ lastUpdated }}
    </div>

    <button
      @click="refreshData"
      class="block mx-auto px-4 py-2 bg-green-600 hover:bg-green-400 text-white rounded transition-colors duration-200"
    >
      Refresh Data
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      lastUpdated: new Date().toLocaleString(),
      stocks: [
        {
          name: "Apple",
          symbol: "AAPL",
          price: 0,
          change: 0,
          changePercent: 0,
        },
        {
          name: "Amazon",
          symbol: "AMZN",
          price: 0,
          change: 0,
          changePercent: 0,
        },
        {
          name: "Google",
          symbol: "GOOGL",
          price: 0,
          change: 0,
          changePercent: 0,
        },
        {
          name: "NVIDIA",
          symbol: "NVDA",
          price: 0,
          change: 0,
          changePercent: 0,
        },
      ],
    };
  },

  mounted() {
    this.fetchStockData();
    this.autoRefresh = setInterval(this.fetchStockData, 60000); // Refresh every 60 seconds
  },

  beforeDestroy() {
    clearInterval(this.autoRefresh);
  },

  methods: {
    async fetchStockData() {
      const apiKey = "ZLH4CQ077J3G5WK8"; // Replace with your Alpha Vantage API key
      const symbols = this.stocks.map((stock) => stock.symbol);
      const requests = symbols.map((symbol) =>
        fetch(
          `https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol=${symbol}&apikey=${apiKey}`
        )
          .then((response) => response.json())
          .then((data) => ({
            symbol,
            price: parseFloat(data["Global Quote"]["05. price"]),
            change: parseFloat(data["Global Quote"]["09. change"]),
            changePercent: parseFloat(
              data["Global Quote"]["10. change percent"]
            ),
          }))
      );

      try {
        const results = await Promise.all(requests);
        this.stocks = this.stocks.map((stock) => {
          const updated = results.find((item) => item.symbol === stock.symbol);
          return {
            ...stock,
            price: updated.price,
            change: updated.change,
            changePercent: updated.changePercent,
          };
        });
        this.lastUpdated = new Date().toLocaleString();
      } catch (error) {
        console.error("Error fetching stock data:", error);
      }
    },

    getBasePrice(symbol) {
      // Return realistic base prices for each stock
      switch (symbol) {
        case "AAPL":
          return 180;
        case "AMZN":
          return 150;
        case "GOOGL":
          return 120;
        case "NVDA":
          return 450;
        default:
          return 100;
      }
    },

    refreshData() {
      this.fetchStockData();
    },
  },
};
</script>

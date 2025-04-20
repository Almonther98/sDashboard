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
  },

  methods: {
    fetchStockData() {
      // In a real application, you would fetch actual data from a stock API
      // For this example, we'll generate random values
      this.stocks.forEach((stock) => {
        // Generate random price based on a realistic base price
        const basePrice = this.getBasePrice(stock.symbol);
        const randomFactor = 0.98 + Math.random() * 0.04; // Random factor between 0.98 and 1.02

        const newPrice = basePrice * randomFactor;
        const oldPrice = stock.price || newPrice * 0.99; // If first load, create small difference
        const priceChange = newPrice - oldPrice;
        const percentChange = (priceChange / oldPrice) * 100;

        stock.price = newPrice;
        stock.change = priceChange;
        stock.changePercent = percentChange;
      });

      this.lastUpdated = new Date().toLocaleString();
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

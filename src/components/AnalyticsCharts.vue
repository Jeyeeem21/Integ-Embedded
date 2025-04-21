<template>
  <div class="analytics-charts">
    <h2>Water Analytics</h2>
    
    <div class="chart-container">
      <div class="daily-chart-wrapper">
        <h3>Daily Cubic Usage - {{ currentMonth }}</h3>
        <div class="scroll-container">
          <canvas ref="dailyChart" class="daily-chart"></canvas>
        </div>
      </div>
      
      <div class="monthly-chart-wrapper">
        <h3>Monthly Cubic Usage</h3>
        <canvas ref="monthlyChart" class="monthly-chart"></canvas>
      </div>
    </div>
  </div>
</template>

<script>
import { Chart, registerables } from 'chart.js';
Chart.register(...registerables);

export default {
  name: 'AnalyticsCharts',
  props: {
    hourlyData: Array,    // Should be an array of values for each hour of day
    dailyData: Array,     // Should be an array of values for each day of month
    monthlyData: Array    // Array of { month: string, cubic: number }
  },
  data() {
    return {
      dailyChartInstance: null,
      monthlyChartInstance: null,
      currentMonth: new Date().toLocaleString('default', { month: 'long' })
    }
  },
  mounted() {
    this.renderCharts();
  },
  watch: {
    dailyData: 'renderCharts',
    monthlyData: 'renderCharts'
  },
  methods: {
    getDailyLabels() {
      const now = new Date();
      const year = now.getFullYear();
      const month = now.getMonth();
      const daysInMonth = new Date(year, month + 1, 0).getDate();
      
      // Generate labels like "1", "2", ..., "31"
      return Array.from({ length: daysInMonth }, (_, i) => `${i + 1}`);
    },
    renderCharts() {
      // Clean up existing charts
      if (this.dailyChartInstance) this.dailyChartInstance.destroy();
      if (this.monthlyChartInstance) this.monthlyChartInstance.destroy();

      // Daily Chart
      this.dailyChartInstance = new Chart(this.$refs.dailyChart, {
        type: 'bar',
        data: {
          labels: this.getDailyLabels(),
          datasets: [{
            label: 'Cubic Measurement',
            data: this.dailyData,
            backgroundColor: 'rgba(76, 175, 80, 0.2)',
            borderColor: '#4caf50',
            borderWidth: 2,
            tension: 0.1,
            fill: true
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            y: {
              beginAtZero: true,
              title: { display: true, text: 'Cubic' }
            },
            x: {
              title: { display: true, text: 'Day of Month' },
              ticks: {
                autoSkip: false,
                maxRotation: 90,
                minRotation: 90
              }
            }
          }
        }
      });

      // Monthly Chart
      this.monthlyChartInstance = new Chart(this.$refs.monthlyChart, {
        type: 'line',
        data: {
          labels: this.monthlyData.map(d => d.month),
          datasets: [{
            label: 'Cubic Measurement',
            data: this.monthlyData.map(d => d.cubic),
            backgroundColor: 'rgba(76, 175, 80, 0.2)',
            borderColor: '#4caf50',
            borderWidth: 2,
            tension: 0.1,
            fill: true
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            y: {
              beginAtZero: true,
              title: { display: true, text: 'Cubic' }
            },
            x: {
              ticks: {
                autoSkip: false,
                maxRotation: 45,
                minRotation: 45
              }
            }
          }
        }
      });
    }
  },
  beforeUnmount() {
    if (this.dailyChartInstance) this.dailyChartInstance.destroy();
    if (this.monthlyChartInstance) this.monthlyChartInstance.destroy();
  }
}
</script>

<style scoped>
.analytics-charts {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  grid-column: span 2;
}

.chart-container {
  display: grid;
  grid-template-columns: 2fr 1fr; /* Daily chart takes more space */
  gap: 20px;
}

.daily-chart-wrapper {
  background: #f9f9f9;
  padding: 15px;
  border-radius: 8px;
  overflow: hidden;
}

.monthly-chart-wrapper {
  background: #f9f9f9;
  padding: 15px;
  border-radius: 8px;
  height: 400px; /* Smaller fixed height */
}

.scroll-container {
  overflow-x: auto;
  width: 100%;
}

.daily-chart {
  min-width: 800px; /* Ensures all bars are visible without squeezing */
  height: 400px;
}

.monthly-chart {
  width: 100% !important;
  height: 100% !important;
}

h3 {
  margin-top: 0;
  color: #555;
  margin-bottom: 15px;
}
</style>
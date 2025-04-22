<template>
  <div class="analytics-charts">
    <h2>Analytics</h2>
    
    <div class="chart-container">
      <div class="chart-wrapper daily-chart-wrapper">
        <div class="chart-header">
          <h3>Daily Cubic Usage - {{ selectedMonthName }} {{ selectedYear }}</h3>
          <div class="chart-controls">
            <select v-model="selectedMonth" @change="updateDailyChart">
              <option v-for="(month, index) in months" :value="index" :key="month">
                {{ month }}
              </option>
            </select>
            <select v-model="selectedYear" @change="updateDailyChart">
              <option v-for="year in availableYears" :value="year" :key="year">
                {{ year }}
              </option>
            </select>
          </div>
        </div>
        <div class="chart-content">
          <canvas ref="dailyChart" class="chart daily-chart"></canvas>
        </div>
      </div>
      
      <div class="chart-wrapper monthly-chart-wrapper">
        <div class="chart-header">
          <h3>Monthly Cubic Usage</h3>
          <select v-model="selectedYearForMonthly" @change="updateMonthlyChart">
            <option v-for="year in availableYears" :value="year" :key="year">
              {{ year }}
            </option>
          </select>
        </div>
        <div class="chart-content">
          <canvas ref="monthlyChart" class="chart monthly-chart"></canvas>
        </div>
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
    hourlyData: Array,
    dailyData: Array,
    monthlyData: Array
  },
  data() {
    const currentDate = new Date();
    return {
      dailyChartInstance: null,
      monthlyChartInstance: null,
      months: [
        'January', 'February', 'March', 'April', 'May', 'June',
        'July', 'August', 'September', 'October', 'November', 'December'
      ],
      selectedMonth: currentDate.getMonth(),
      selectedYear: currentDate.getFullYear(),
      selectedYearForMonthly: currentDate.getFullYear(),
      availableYears: this.generateAvailableYears()
    }
  },
  computed: {
    selectedMonthName() {
      return this.months[this.selectedMonth];
    },
    currentMonth() {
      return new Date().toLocaleString('default', { month: 'long' });
    }
  },
  mounted() {
    this.renderCharts();
    window.addEventListener('resize', this.handleResize);
  },
  beforeUnmount() {
    if (this.dailyChartInstance) this.dailyChartInstance.destroy();
    if (this.monthlyChartInstance) this.monthlyChartInstance.destroy();
    window.removeEventListener('resize', this.handleResize);
  },
  watch: {
    dailyData: 'renderCharts',
    monthlyData: 'renderCharts'
  },
  methods: {
    generateAvailableYears() {
      const currentYear = new Date().getFullYear();
      const years = [];
      for (let year = currentYear; year >= currentYear - 5; year--) {
        years.push(year);
      }
      return years;
    },
    
    handleResize() {
      if (this.resizeTimer) clearTimeout(this.resizeTimer);
      this.resizeTimer = setTimeout(() => {
        this.renderCharts();
      }, 250);
    },
    
    getDailyLabels() {
      const year = this.selectedYear;
      const daysInMonth = new Date(year, this.selectedMonth + 1, 0).getDate();
      return Array.from({ length: daysInMonth }, (_, i) => `${i + 1}`);
    },
    
    updateDailyChart() {
      if (this.dailyChartInstance) {
        this.dailyChartInstance.data.labels = this.getDailyLabels();
        // Keep using the original dailyData without filtering
        this.dailyChartInstance.data.datasets[0].data = this.dailyData;
        this.dailyChartInstance.update();
      }
    },
    
    updateMonthlyChart() {
      if (this.monthlyChartInstance) {
        // Keep using the original monthlyData without filtering
        this.monthlyChartInstance.data.labels = this.monthlyData.map(d => d.month);
        this.monthlyChartInstance.data.datasets[0].data = this.monthlyData.map(d => d.cubic);
        this.monthlyChartInstance.update();
      }
    },
    
    getDailyChartOptions(isMobile) {
      return {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            display: false
          },
          tooltip: {
            enabled: true,
            mode: 'index',
            intersect: false,
            callbacks: {
              title: (tooltipItems) => `Day ${tooltipItems[0].label}`,
              label: (context) => `Cubic: ${context.raw}`
            }
          }
        },
        scales: {
          y: {
            beginAtZero: true,
            title: { 
              display: !isMobile,
              text: 'Cubic' 
            },
            ticks: {
              font: {
                size: isMobile ? 10 : 12
              }
            }
          },
          x: {
            title: { 
              display: !isMobile,
              text: 'Day of Month'
            },
            ticks: {
              maxRotation: 90,
              minRotation: 90,
              font: {
                size: isMobile ? 9 : 11
              },
              autoSkip: false
            }
          }
        },
        layout: {
          padding: {
            bottom: isMobile ? 15 : 25
          }
        }
      };
    },
    
    getMonthlyChartOptions(isMobile) {
      return {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            display: false
          },
          tooltip: {
            enabled: true,
            mode: 'index',
            intersect: false,
            callbacks: {
              label: (context) => `Cubic: ${context.raw}`
            }
          }
        },
        scales: {
          y: {
            beginAtZero: true,
            title: { 
              display: !isMobile,
              text: 'Cubic' 
            },
            ticks: {
              font: {
                size: isMobile ? 10 : 12
              }
            }
          },
          x: {
            title: { 
              display: !isMobile,
              text: 'Month'
            },
            ticks: {
              maxRotation: 45,
              minRotation: 45,
              font: {
                size: isMobile ? 9 : 11
              },
              autoSkip: false
            }
          }
        },
        layout: {
          padding: {
            bottom: isMobile ? 15 : 10
          }
        }
      };
    },
    
    renderCharts() {
      if (this.dailyChartInstance) this.dailyChartInstance.destroy();
      if (this.monthlyChartInstance) this.monthlyChartInstance.destroy();
      
      const isMobile = window.innerWidth < 768;

      // Daily Chart
      this.dailyChartInstance = new Chart(this.$refs.dailyChart, {
        type: 'bar',
        data: {
          labels: this.getDailyLabels(),
          datasets: [{
            label: 'Cubic Measurement',
            data: this.dailyData, // Using original data without filtering
            backgroundColor: 'rgba(76, 175, 80, 0.2)',
            borderColor: '#4caf50',
            borderWidth: isMobile ? 1 : 2,
            tension: 0.1,
            fill: true
          }]
        },
        options: this.getDailyChartOptions(isMobile)
      });

      // Monthly Chart
      this.monthlyChartInstance = new Chart(this.$refs.monthlyChart, {
        type: 'line',
        data: {
          labels: this.monthlyData.map(d => d.month), // Using original data
          datasets: [{
            label: 'Cubic Measurement',
            data: this.monthlyData.map(d => d.cubic), // Using original data
            backgroundColor: 'rgba(76, 175, 80, 0.2)',
            borderColor: '#4caf50',
            borderWidth: isMobile ? 1 : 2,
            tension: 0.1,
            fill: true
          }]
        },
        options: this.getMonthlyChartOptions(isMobile)
      });
    }
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
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.chart-wrapper {
  background: #f9f9f9;
  padding: 15px;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
}
.chart-controls.mobile-right {
  justify-content: flex-end; /* Align controls to the right in mobile view */
}
.chart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
  flex-wrap: wrap;
  gap: 10px;
}

.chart-header h3 {
  margin: 0;
}

.chart-controls {
  display: flex;
  gap: 10px;
}

.chart-header select {
  padding: 5px 10px;
  border-radius: 4px;
  border: 1px solid #ddd;
  background-color: white;
  font-size: 12px;
}

.chart-content {
  flex: 1;
  position: relative;
  min-height: 280px;
}

.chart {
  position: absolute;
  top: 0;
  left: 0;
  width: 100% !important;
  height: 100% !important;
}

h2 {
  color: #2c3e50;
  margin-top: 0;
  margin-bottom: 20px;
  font-size: 1.5rem;
}

h3 {
  color: #555;
  font-size: 1rem;
}

.daily-chart-wrapper .chart-content {
  overflow-x: auto;
}

.daily-chart-wrapper .chart {
  min-width: 500px;
}

@media (min-width: 768px) {
  .chart-content {
    min-height: 320px;
  }
  
  .daily-chart-wrapper .chart {
    min-width: 600px;
  }
  
  h2 {
    font-size: 1.75rem;
  }
  
  h3, .chart-header select {
    font-size: 1.1rem;
  }
}

@media (min-width: 1024px) {
  .chart-container {
    flex-direction: row;
    
  }
  
  .daily-chart-wrapper {
    flex: 2;
  }
  
  .monthly-chart-wrapper {
    flex: 1;
  }
  
  .chart-content {
    min-height: 400px;
  }
  
  .daily-chart-wrapper .chart {
    min-width: 800px;
  }
  
  h2 {
    font-size: 2rem;
  }
  
  h3, .chart-header select {
    font-size: 15px;
  }
}
</style>
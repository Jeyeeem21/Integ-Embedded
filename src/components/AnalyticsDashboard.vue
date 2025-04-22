<template>
  <div class="dashboard">
    <h1>Analytics Dashboard</h1>
    
    <div class="dashboard-grid">
      <AnalyticsCharts 
     
        :dailyData="dailyAnalytics"
        :monthlyData="monthlyAnalytics" 
      />
      
      <DataTable :items="tableData" />
    </div>
  </div>
</template>

<script>
import AnalyticsCharts from './AnalyticsCharts.vue';
import DataTable from './DataTable.vue';

export default {
  name: 'AnalyticsDashboard',
  components: {
    AnalyticsCharts,
    DataTable
  },
  data() {
    return {
      
      
      // Daily data for current month
      dailyAnalytics: (() => {
        const now = new Date();
        const daysInMonth = new Date(now.getFullYear(), now.getMonth() + 1, 0).getDate();
        return Array.from({ length: daysInMonth }, (_, i) => 
          Math.floor(Math.random() * 10) 
        );
      })(),
      
      // Monthly data
      monthlyAnalytics: [
        { month: 'January', cubic: 45 },
        { month: 'February', cubic: 38 },
        { month: 'March', cubic: 52 },
        { month: 'April', cubic: 41 },
        { month: 'May', cubic: 49 },
        { month: 'June', cubic: 56 }
      ],
      
      // Table data
      tableData: []
    }
  },
  created() {
    this.updateTableData(); 
    this.startRealTimeUpdates(); 
  },
  methods: {
    updateTableData() {
      const now = new Date();
      const currentMonth = now.toLocaleString('default', { month: 'long' });
      const year = now.getFullYear();
      
      // Generate table data from dailyAnalytics
      this.tableData = this.dailyAnalytics.map((cubic, index) => ({
        id: index + 1,
        name: Item ${index + 1},
        address: ${index + 1} Main St,
        currentCubic: cubic,
        totalCubic: this.dailyAnalytics.slice(0, index + 1).reduce((sum, val) => sum + val, 0),
        date: ${year}-${String(now.getMonth() + 1).padStart(2, '0')}-${String(index + 1).padStart(2, '0')}
      }));
    },
    startRealTimeUpdates() {
      setInterval(() => {
        // Simulate fetching new daily data
        this.dailyAnalytics = this.dailyAnalytics.map(() => Math.floor(Math.random() * 10));
        this.updateTableData(); // Update table based on new data
      }, 5000); // Update every 5 seconds
    }
  }
}
</script>

<style scoped>
.dashboard {
  padding: 20px;
  font-family: Arial, sans-serif;
}

.dashboard-grid {
  display: grid;
  grid-template-columns: 1fr;
  grid-gap: 20px;
}

h1 {
  color: #2c3e50;
  margin-bottom: 20px;
}

.dashboard-grid {
  display: grid;
  grid-template-columns: 1fr;
  grid-gap: 20px;
}

/* Responsive styles */
@media (min-width: 1024px) {
  .dashboard-grid {
    grid-template-columns: 1fr;
  }
  
  .dashboard {
    padding: 30px;
  }
}
</style>
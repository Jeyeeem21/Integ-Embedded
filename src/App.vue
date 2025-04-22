<template>
  <div class="app">
    
    <Header />
    
    <div class="dashboard">
      <div class="dashboard-grid">
        <!-- Analytics Charts Section -->
        <AnalyticsCharts 
          :dailyData="dailyCubicUsage" 
          :monthlyData="monthlyAnalytics" 
        />
        
        <!-- Data Table Section -->
        <DataTable :items="tableData" />
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import AnalyticsCharts from './components/AnalyticsCharts.vue'
import DataTable from './components/DataTable.vue'

export default {
  components: {
    Header,
    AnalyticsCharts,
    DataTable
  },
  data() {
    return {
    
      
      // Sample analytics data
      dailyAnalytics: [
  { day: '1', cubic: 3 },
  { day: '2', cubic: 5 },
  { day: '3', cubic: 2 },
  { day: '4', cubic: 4 },
  { day: '5', cubic: 6 },
  { day: '6', cubic: 1 },
  { day: '7', cubic: 0 },
  { day: '8', cubic: 10 },
  { day: '9', cubic: 13 },
  { day: '10', cubic: 2 },
  { day: '11', cubic: 3 },
  { day: '12', cubic: 5 },
  { day: '13', cubic: 1 },
  { day: '14', cubic: 8 },
  { day: '15', cubic: 1 },
  { day: '16', cubic: 2 },
  { day: '17', cubic: 6 },
  { day: '18', cubic: 4 },
  { day: '19', cubic: 7 },
  { day: '20', cubic: 5 },
  { day: '21', cubic: 9 },
  { day: '22', cubic: 3 },
  { day: '23', cubic: 4 },
  { day: '24', cubic: 2 },
  { day: '25', cubic: 8 },
  { day: '26', cubic: 5 },
  { day: '27', cubic: 6 },
  { day: '28', cubic: 3 },
  { day: '29', cubic: 2 },
  { day: '30', cubic: 7 }
],

      
      monthlyAnalytics: [
  { month: 'Jan', cubic: 10 },
  { month: 'Feb', cubic: 38 },
  { month: 'Mar', cubic: 52 },
  { month: 'Apr', cubic: 41 },
  { month: 'May', cubic: 49 },
  { month: 'Jun', cubic: 56 },
  { month: 'Jul', cubic: 62 },
  { month: 'Aug', cubic: 44 },
  { month: 'Sep', cubic: 39 },
  { month: 'Oct', cubic: 48 },
  { month: 'Nov', cubic: 53 },
  { month: 'Dec', cubic: 60 }
],

      
      // Sample table data
      tableData: [
  { id: 'House 1', name: 'JM', address: 'Cainta blk1', currentCubic: 1, totalCubic: 5, date: '2023-05-01' },
  { id: 'House 2', name: 'Jho', address: 'Cainta blk2', currentCubic: 2, totalCubic: 8, date: '2023-05-02' },
  { id: 'House 3', name: 'Nila', address: 'Cainta blk3', currentCubic: 1, totalCubic: 3, date: '2023-05-03' },
  { id: 'House 4', name: 'Baldo', address: 'Cainta blk5', currentCubic: 3, totalCubic: 10, date: '2023-05-04' },
  { id: 'House 5', name: 'Nancy', address: 'Cainta blk7', currentCubic: 1, totalCubic: 6, date: '2023-05-05' },
  { id: 'House 6', name: 'Leo', address: 'Cainta blk8', currentCubic: 2, totalCubic: 7, date: '2023-05-06' },
  { id: 'House 7', name: 'Marco', address: 'Cainta blk9', currentCubic: 1, totalCubic: 4, date: '2023-05-07' },
  { id: 'House 8', name: 'Lisa', address: 'Cainta blk10', currentCubic: 2, totalCubic: 6, date: '2023-05-08' },
  { id: 'House 9', name: 'Gio', address: 'Cainta blk11', currentCubic: 3, totalCubic: 9, date: '2023-05-09' },
  { id: 'House 10', name: 'Eva', address: 'Cainta blk12', currentCubic: 1, totalCubic: 2, date: '2023-05-10' },
  { id: 'House 11', name: 'Mara', address: 'Cainta blk13', currentCubic: 2, totalCubic: 5, date: '2023-05-11' },
  { id: 'House 12', name: 'Rico', address: 'Cainta blk14', currentCubic: 1, totalCubic: 7, date: '2023-05-12' },
  { id: 'House 13', name: 'Jona', address: 'Cainta blk15', currentCubic: 3, totalCubic: 8, date: '2023-05-13' },
  { id: 'House 14', name: 'Carl', address: 'Cainta blk16', currentCubic: 2, totalCubic: 4, date: '2023-05-14' },
  { id: 'House 15', name: 'Mitch', address: 'Cainta blk17', currentCubic: 1, totalCubic: 6, date: '2023-05-15' }
]

    }
  },
  computed: {
    dailyCubicUsage() {
      const now = new Date();
      const year = now.getFullYear();
      const month = now.getMonth();
      const daysInMonth = new Date(year, month + 1, 0).getDate();

      // Create an array of zeros for all days in the current month
      const dailyUsage = Array(daysInMonth).fill(0);

      // Map the provided data to the corresponding day indices
      this.dailyAnalytics.forEach(entry => {
        const dayIndex = parseInt(entry.day, 10) - 1; // Convert "1" (day) to index 0
        if (dayIndex >= 0 && dayIndex < daysInMonth) {
          dailyUsage[dayIndex] = entry.cubic;
        }
      });

      return dailyUsage;
    }
  }
}
</script>

<style>
/* Global styles */
body {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
  background-color: #f5f7fa;
}

.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.dashboard {
  flex: 1;
  padding: 20px;
  max-width: 1400px;
  margin: 0 auto;
  width: 100%;
  box-sizing: border-box;
}

.dashboard-grid {
  display: grid;
  grid-template-columns: 1fr;
  grid-gap: 20px;
}

/* Responsive styles */
@media (min-width: 1024px) {
  .dashboard {
    padding: 30px;
  }
}
</style>
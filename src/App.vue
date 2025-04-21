<template>
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
</template>

<script>
import AnalyticsCharts from './components/AnalyticsCharts.vue'
import DataTable from './components/DataTable.vue'

export default {
  components: {
    AnalyticsCharts,
    DataTable
  },
  data() {
    return {
      currentBucketCubic: 1, // 1 cubic in bucket
      totalDrumCubic: 15,   // total cubic in drum
      
      // Sample analytics data
      dailyAnalytics: [
        { day: '1', cubic: 3 },  // Day 1
        { day: '2', cubic: 5 },  // Day 2
        { day: '3', cubic: 2 },  // Day 3
        { day: '4', cubic: 4 },  // Day 4
        { day: '5', cubic: 6 },  // Day 5
        { day: '6', cubic: 1 },  // Day 6
        { day: '7', cubic: 0 },   // Day 7
        { day: '8', cubic: 10 },
        { day: '9', cubic: 13 },  // Day 1
        { day: '10', cubic: 2 },  // Day 2
        { day: '11', cubic: 3 },  // Day 3
        { day: '12', cubic: 5 },  // Day 4
        { day: '13', cubic: 1 },  // Day 5
        { day: '14', cubic: 8 },  // Day 6
        { day: '15', cubic: 1 },   // Day 7
        { day: '16', cubic: 2 }
      ],
      
      monthlyAnalytics: [
        { month: 'Jan', cubic: 20 },
        { month: 'Feb', cubic: 38 },
        { month: 'Mar', cubic: 52 },
        { month: 'Apr', cubic: 41 },
        { month: 'May', cubic: 49 },
        { month: 'Jun', cubic: 56 }
      ],
      
      // Sample table data
      tableData: [
  { id: 'House 1', name: 'JM', address: 'Cainta blk1', currentCubic: 1, totalCubic: 5, date: '2023-05-01' },
  { id: 'House 2', name: 'Jho', address: 'Cainta blk2', currentCubic: 2, totalCubic: 8, date: '2023-05-02' },
  { id: 'House 3', name: 'Nila', address: 'Cainta blk3', currentCubic: 1, totalCubic: 3, date: '2023-05-03' },
  { id: 'House 4', name: 'Baldo', address: 'Cainta blk5', currentCubic: 3, totalCubic: 10, date: '2023-05-04' },
  { id: 'House 5', name: 'Nancy', address: 'Cainta blk7', currentCubic: 1, totalCubic: 6, date: '2023-05-05' }
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
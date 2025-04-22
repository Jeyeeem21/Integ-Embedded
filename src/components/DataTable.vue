<template>
<div class="table-responsive">
    <h2>Data Records</h2>
    
    <table>
      <thead>
        <tr>
          <th>House ID</th>
          <th>Name</th>
          <th>Address</th>
          <th>Current Cubic</th>
          <th>Total Cubic</th>
          <th>Date</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in items" :key="item.id">
          <td>{{ item.id }}</td>
          <td>{{ item.name }}</td>
          <td>{{ item.address }}</td>
          <td>{{ item.currentCubic }}</td>
          <td>{{ item.totalCubic }}</td>
          <td>{{ item.date }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    items: Array
  }
}
</script>

<style scoped>
.data-table {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  grid-column: span 2;
}

h2 {
  color: #2c3e50;
  margin-top: 0;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

th, td {
  padding: 12px 15px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

th {
  background-color: #f5f5f5;
  font-weight: bold;
}

tr:hover {
  background-color: #f9f9f9;
}
.table-responsive {
  overflow-x: auto;
  width: 100%;
}

/* Adjust padding for smaller screens */
@media (max-width: 768px) {
  .data-table {
    padding: 15px;
  }
  
  th, td {
    padding: 8px 10px;
  }
}
</style>
<template>
  <div class="dashboard-container">
    <!-- <component :is="currentRole" /> -->
    <h2>Dashboard</h2>
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  data() {
    return {
      currentRole: 'adminDashboard'
    }
  },
}
</script>

<template>
  <div class="d-flex" id="app-layout">
    <!-- Sidebar -->
    <nav class="bg-light sidebar p-3">
      <h5 class="mb-4">EnergyCo</h5>
      <ul class="nav flex-column">
        <li class="nav-item">
          <a class="nav-link active" href="#">Upload Readings</a>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled" href="#">Reports</a>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled" href="#">Billing</a>
        </li>
      </ul>
    </nav>

    <!-- Main Content -->
    <div class="flex-grow-1">
      <header class="bg-primary text-white p-3">
        <h3 class="m-0">EnergyCo Admin Portal</h3>
      </header>

      <main class="p-4">
        <EnergyUploader />
      </main>
    </div>
  </div>
</template>

<script>
import EnergyUploader from "./components/EnergyUploader.vue";

export default {
  name: "App",
  components: {
    EnergyUploader,
  },
};
</script>

<style>
#app-layout {
  min-height: 100vh;
}

.sidebar {
  width: 220px;
  border-right: 1px solid #dee2e6;
}

.nav-link.active {
  font-weight: bold;
  color: #0d6efd;
}
</style>

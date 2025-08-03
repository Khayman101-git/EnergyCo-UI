<template>
  <div class="container mt-5">
    <div class="card shadow rounded-4">
      <div class="card-body">
        <h3 class="card-title mb-4">Customer Meter Reading Upload</h3>

        <div class="mb-3">
          <label for="csvFile" class="form-label">Upload CSV File</label>
          <input
            class="form-control"
            type="file"
            id="csvFile"
            accept=".csv"
            @change="handleFileChange"
          />
        </div>

        <button
          class="btn btn-primary"
          :disabled="!csvFile || isLoading"
          @click="submitFile"
        >
          {{ isLoading ? 'Uploading...' : 'Submit Readings' }}
        </button>

        <div v-if="message" class="alert mt-4" :class="messageClass">
          {{ message }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EnergyUploader",
  data() {
    return {
      csvFile: null,
      message: "",
      messageType: "",
      isLoading: false,
    };
  },
  computed: {
    messageClass() {
      return {
        "alert-success": this.messageType === "success",
        "alert-danger": this.messageType === "error",
      };
    },
  },
  methods: {
    handleFileChange(event) {
      const file = event.target.files[0];
      if (file && file.type === "text/csv") {
        this.csvFile = file;
        this.message = "";
      } else {
        this.message = "Please upload a valid CSV file.";
        this.messageType = "error";
        this.csvFile = null;
      }
    },
    async submitFile() {
      if (!this.csvFile) return;

      this.isLoading = true;
      this.message = "";

      try {
        const formData = new FormData();
        formData.append("file", this.csvFile);

        // Replace with your actual endpoint
        const response = await fetch("https://localhost:7082/meter-reading-uploads", {
          method: "POST",
          body: formData,
        });

        const result = await response.json();

        if (response.ok) {
          this.message = result.message || "File uploaded successfully";
          this.messageType = "success";
          this.message = `${this.message}. ${result.failedReadings} records not updated. ${result.successfulReadings} records updated. `
        } else {
          this.message = result.message || "Upload failed.";
          this.messageType = "error";
        }
      } catch (error) {
        this.message = "An error occurred while uploading the file.";
        this.messageType = "error";
      } finally {
        this.isLoading = false;
      }
    },
  },
};
</script>

<style scoped>
.card {
  max-width: 600px;
  margin: auto;
}
</style>

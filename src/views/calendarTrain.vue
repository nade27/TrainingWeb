<template>
    <div class="p-4">
      <!-- Tombol Tambah Training -->
      <button @click="showTrainingDialog = true" class="mb-4 bg-blue-500 text-white px-4 py-2 rounded">
        Tambah Training
      </button>
  
      <!-- Komponen Kalender -->
      <vue-cal
        :events="events"
        :disable-views="['years', 'year', 'week', 'day']"
        view="month"
        :style-cal="{ color: 'black', borderColor: 'black' }"
      />
  
      <!-- Popup Form untuk Menambah Training -->
      <div v-if="showTrainingDialog" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
        <div class="p-6 bg-white rounded-lg shadow-lg max-w-md mx-auto">
          <h2 class="text-lg font-bold mb-4">Tambah Training</h2>
  
          <!-- Form Input -->
          <div class="mb-2">
            <label class="block text-sm font-semibold">Topik Training</label>
            <input v-model="newTraining.topic" type="text" class="border p-2 w-full rounded" />
          </div>
  
          <div class="mb-2">
            <label class="block text-sm font-semibold">Tanggal Mulai - Selesai</label>
            <input v-model="newTraining.startDate" type="date" class="border p-2 w-full rounded" />
            <input v-model="newTraining.endDate" type="date" class="border p-2 w-full rounded mt-1" />
          </div>
  
          <div class="mb-2">
            <label class="block text-sm font-semibold">Venue</label>
            <input v-model="newTraining.venue" type="text" class="border p-2 w-full rounded" />
          </div>
  
          <div class="mb-2">
            <label class="block text-sm font-semibold">Durasi (jam)</label>
            <input v-model="newTraining.duration" type="number" class="border p-2 w-full rounded" />
          </div>
  
          <div class="mb-2">
            <label class="block text-sm font-semibold">Requirement</label>
            <input v-model="newTraining.requirement" type="text" class="border p-2 w-full rounded" />
          </div>
  
          <div class="mb-2">
            <label class="block text-sm font-semibold">Enrollment</label>
            <input v-model="newTraining.enrollment" type="text" class="border p-2 w-full rounded" />
          </div>
  
          <!-- Tombol Simpan dan Batal -->
          <div class="flex justify-end mt-4">
            <button @click="addTraining" class="bg-green-500 text-white px-4 py-2 rounded mr-2">
              Simpan
            </button>
            <button @click="showTrainingDialog = false" class="bg-gray-400 text-white px-4 py-2 rounded">
              Batal
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from "vue";
  import axios from "axios";
  import "vue-cal/dist/vuecal.css";
  import VueCal from "vue-cal";
  
  const events = ref([]);
  const showTrainingDialog = ref(false);
  const trainings = ref([]);
  
  const newTraining = ref({
    topic: "",
    startDate: "",
    endDate: "",
    venue: "",
    duration: "",
    requirement: "",
    enrollment: "",
  });
  
  const fetchTrainings = async () => {
    try {
      const response = await axios.get("http://localhost:3000/training");
      trainings.value = response.data;
      events.value = response.data.map((t) => ({
        start: t.startDate,
        end: t.endDate,
        title: t.topic,
      }));
    } catch (error) {
      console.error("Gagal mengambil data training:", error);
    }
  };
  
  const addTraining = async () => {
    if (newTraining.value.topic.trim()) {
      try {
        await axios.post("http://localhost:3000/training", newTraining.value);
        fetchTrainings();
      } catch (error) {
        console.error("Gagal menyimpan data training:", error);
      }
  
      // Reset form
      newTraining.value = {
        topic: "",
        startDate: "",
        endDate: "",
        venue: "",
        duration: "",
        requirement: "",
        enrollment: "",
      };
  
      // Tutup dialog
      showTrainingDialog.value = false;
    }
  };
  
  onMounted(fetchTrainings);
  </script>
  
  <style>
  /* Styling tambahan untuk kalender */
  .vuecal {
    color: black !important;
  }
  
  /* Ukuran sel kalender */
  .vuecal__cell {
    min-width: 120px !important;
    min-height: 160px !important;
    position: relative;
    text-align: left;
    padding: 5px;
  }
  
  /* Posisi angka tanggal di pojok kanan atas */
  .vuecal__cell-content .vuecal__cell-date {
    position: absolute;
    top: 5px;
    right: 5px;
    font-size: 14px !important;
    font-weight: bold;
  }
  
  /* Highlight tanggal hari ini */
  .vuecal__cell--today {
    background-color: yellow !important;
    color: black !important;
    font-weight: bold;
    border-radius: 5px;
  }
  </style>
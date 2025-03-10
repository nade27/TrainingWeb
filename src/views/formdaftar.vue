// src/App.vue
<template>
  <div class="flex justify-center items-center min-h-screen bg-gray-100 flex-col p-6">
    <div class="bg-white p-6 pr-30 pl-30 rounded-lg shadow-lg w-full max-w-3xl">
      <h2 class="text-center text-xl font-bold mb-4 text-black">Input Data Training</h2>
      <form @submit.prevent="addData" class="space-y-4">
        <div class="relative z-0 w-full group" v-for="(label, key) in formLabels" :key="key">
          <input :type="getInputType(key)" v-model="form[key]" :id="key" class="block py-2.5 px-0 w-full text-sm text-black bg-transparent border-0 border-b-2 border-black focus:outline-none focus:ring-0 focus:border-blue-600 peer" placeholder=" " required />
          <label :for="key" class="absolute text-sm text-black duration-300 transform -translate-y-6 scale-75 top-3 -z-10 origin-[0] peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-6 peer-focus:text-blue-600">{{ label }}</label>
        </div>
        <button type="button" @click="addData" class="w-full bg-green-500 text-white p-2 rounded hover:bg-green-600" :disabled="isAdding">
          {{ isAdding ? 'Menambahkan...' : 'Add' }}
        </button>
      </form>
    </div>

    <div class="bg-white p-6 rounded-lg shadow-lg w-full max-w-5xl mt-6">
      <h2 class="text-center text-xl font-bold mb-4">Data Training</h2>
      <table v-show="true"class="w-full border-collapse border border-black text-sm text-black">
        <thead>
          <tr class="bg-gray-300 text-black text-center font-bold">
            <th class="border border-black p-3" v-for="(label, key) in formLabels" :key="key">{{ label }}</th>
            <th class="border border-black p-3">Aksi</th>
          </tr>
        </thead>
        <tbody>
              <!-- Render data jika ada -->
            <tr v-for="(item, index) in dataList" :key="index" class="text-center">
              <td class="border border-black p-3" v-for="(label, key) in formLabels" :key="key">{{ item[key] }}</td>
              <td class="border border-black p-3">
                <button @click="removeData(index)" class="bg-red-500 text-white p-1 rounded hover:bg-red-600">Hapus</button>
              </td>
            </tr>

              <!-- Render baris kosong jika tidak ada data -->
            <tr v-if="dataList.length === 0">
              <td class="border border-black p-3 text-center text-gray-500 italic" :colspan="Object.keys(formLabels).length + 1">
                Tidak ada data
              </td>
            </tr>
         </tbody>
      </table>


      <button @click="submitForm" class="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600 mt-4" :disabled="isSubmitting">
        {{ isSubmitting ? 'Mengirim...' : 'Kirim' }}
      </button>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import Swal from 'sweetalert2';

export default {
  setup() {
    const form = ref({
      NIP: '', NamaLengkap: '', Department: '', Grade: '', Jabatan: '',
      CodeCenter: '', TopikTraining: '', TanggalTraining: '', EmailPeserta: '', NomorWhatsAppPeserta: ''
    });
    
    const formLabels = ref({
      NIP: 'NIP', NamaLengkap: 'Nama Lengkap', Department: 'Department', Grade: 'Grade', Jabatan: 'Jabatan',
      CodeCenter: 'Code Center', TopikTraining: 'Topik Training', TanggalTraining: 'Tanggal Training',
      EmailPeserta: 'Email Peserta', NomorWhatsAppPeserta: 'Nomor WhatsApp'
    });

    const dataList = ref([]);
    const isAdding = ref(false);
    const isSubmitting = ref(false);

    const getInputType = (key) => {
      if (key === 'EmailPeserta') return 'email';
      if (key === 'TanggalTraining') return 'date';
      return 'text';
    };

    const addData = () => {
      isAdding.value = true;
      for (let key in form.value) {
        if (!form.value[key]) {
          Swal.fire("Error", `Field ${formLabels.value[key]} tidak boleh kosong`, "error");
          isAdding.value = false;
          return;
        }
      }
      dataList.value.push(JSON.parse(JSON.stringify(form.value)));
      Object.keys(form.value).forEach(key => form.value[key] = '');
      isAdding.value = false;
    };

    const removeData = (index) => {
      dataList.value.splice(index, 1);
    };

    const submitForm = async () => {
      isSubmitting.value = true;
      console.log("Data sebelum dikirim:", dataList.value);
      try {
        const response = await fetch('http://localhost:3000/training', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(dataList.value)
        });

        if (response.ok) {
          Swal.fire("Sukses", "Data berhasil dikirim!", "success");
          dataList.value = [];
        } else {
          Swal.fire("Error", "Gagal mengirim data", "error");
        }
      } catch (error) {
        console.error('Error:', error);
        Swal.fire("Error", "Terjadi kesalahan saat mengirim data", "error");
      }
      isSubmitting.value = false;
    };

    return { form, formLabels, dataList, addData, removeData, submitForm, getInputType, isAdding, isSubmitting };
  }
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
}
</style>

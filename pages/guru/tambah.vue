<template>
  <div class="container-fluid">
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css"
    />
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">ISI KEHADIRAN</h2>
        <nuxt-link to="/halaman1">
          <i class="bi bi-arrow-left"></i>
        </nuxt-link>
        <form @submit.prevent="kirimData" aria-labelledby="form-title">
          <h3 id="form-title" class="sr-only">Formulir Pengisian Kehadiran</h3> <!-- Untuk pembaca layar -->
          
          <div class="mb-3">
            <label for="nama" class="form-label" aria-label="Pilih nama siswa">NAMA</label>
            <select 
              v-model="form.nama"
              id="nama"
              class="form-control form-control-lg form-select rounded-5"
              aria-placeholder="Pilih nama siswa"
              required
              tabindex="1"
            >
              <option value="">Pilih Nama</option>
              <option v-for="(member, i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
            </select>
          </div>
          
          <div class="mb-3">
            <label for="keterangan" class="form-label" aria-label="Pilih keterangan kehadiran">KETERANGAN</label>
            <select 
              v-model="form.keterangan"
              id="keterangan"
              class="form-control form-control-lg form-select rounded-5"
              aria-placeholder="Pilih jenis kehadiran"
              required
              tabindex="2"
            >
              <option value="">Pilih Keterangan</option>
              <option v-for="(objective, i) in objectives" :key="i" :value="objective.id">{{ objective.keterangan }}</option>
            </select>
          </div>

          <button 
            type="submit" 
            class="btn btn-lg rounded-5 px-5 bg-success text-white" 
            style="float: left;"
            aria-live="polite"
            tabindex="3"
          >
            Kirim
          </button>
        </form>

        <!-- Pesan konfirmasi setelah form berhasil dikirim -->
        <div v-if="isFormSubmitted" class="alert alert-success mt-3" role="alert" aria-live="assertive">
          Data berhasil dikirim!
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();

const members = ref([]);
const objectives = ref([]);
const form = ref({
  nama: "",
  keterangan: "",
});
const isFormSubmitted = ref(false);  // Untuk melacak apakah form telah disubmit

const kirimData = async () => {
  const { data, error } = await supabase.from("riwayat").insert([form.value]);

  if (error) {
    console.error('Error submitting data:', error);
    return;
  }
  
  // Set isFormSubmitted untuk menampilkan pesan konfirmasi
  isFormSubmitted.value = true;

  // Reset form setelah submit
  form.value = {
    nama: "",
    keterangan: "",
  };

  // Arahkan ke halaman lain (opsional)
  setTimeout(() => {
    navigateTo('/reri/tambah');
  }, 1500); // Timeout agar pengguna dapat melihat pesan konfirmasi
};

const getNama = async () => {
  const { data, error } = await supabase.from('siswa').select('*');
  if (data) members.value = data;
};

const getKeterangan = async () => {
  const { data, error } = await supabase.from('keterangan').select('*');
  if (data) objectives.value = data;
};

onMounted(() => {
  getNama();
  getKeterangan();
});
</script>

<style scoped>
.btn.bg-success {
  background-color: rgb(108, 50, 163) !important;
}

i {
  color: black;
  font-size: 2em;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  border: 0;
  clip: rect(0, 0, 0, 0);
  overflow: hidden;
}

@media (max-width: 768px) {
  .card {
    padding: 20px;
  }
}
</style>

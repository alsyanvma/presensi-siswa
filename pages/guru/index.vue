<template>
  <div class="container-fluid">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    <div class="row">
      <div class="col-lg-12">
        <!-- Judul halaman dengan ID untuk referensi -->
        <h2 class="text-center my-4" id="dataSiswaTitle">DATA SISWA</h2>

        <!-- Link Kembali dengan Deskripsi Aksesibilitas -->
        <nuxt-link to="/halaman1" aria-label="Kembali ke halaman sebelumnya">
          <i class="bi bi-arrow-left"></i>
        </nuxt-link>

        <!-- Form Pencarian dengan Label yang Jelas -->
        <div class="my-3">
          <form @submit.prevent="getguru">
            <label for="searchInput" class="form-label">Cari Nama Siswa</label>
            <input
              id="searchInput"
              v-model="keyword"
              type="search"
              class="form-control rounded-5"
              placeholder="Cari nama siswa"
              aria-label="Cari nama siswa"
            />
          </form>
        </div>

        <!-- Tampilkan jumlah data siswa -->
        <div class="my-3 text-muted">Menampilkan {{ filteredVisitors.length }} dari {{ jumlah }} siswa</div>

        <!-- Tampilkan pesan jika tidak ada hasil pencarian -->
        <div v-if="filteredVisitors.length === 0" class="text-center text-danger">
          Nama tidak ada
        </div>

        <!-- Tabel dengan Aksesibilitas yang Lebih Baik -->
        <div class="table-responsive">
          <table class="table table-bordered" aria-labelledby="dataSiswaTitle">
            <thead>
              <tr>
                <th scope="col">NO</th>
                <th scope="col">NIS</th>
                <th scope="col">NAMA</th>
                <th scope="col">KELAS</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(visitor, i) in filteredVisitors" :key="i">
                <td>{{ i + 1 }}.</td>
                <td>{{ visitor.nis }}</td>
                <td>{{ visitor.nama }}</td>
                <td>{{ visitor.kelas }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>


<script setup>
import { ref, computed, onMounted } from "vue";
const supabase = useSupabaseClient();
const keyword = ref('');
const visitors = ref([]);
const jumlah = ref([]);

// Ambil data siswa
const getsiswa = async () => {
  const { data, error } = await supabase.from('siswa').select('*');
  if (data) visitors.value = data;
};

// Ambil jumlah siswa
const getjumlah = async () => {
  const { data, count } = await supabase.from('siswa').select("*", { count: 'exact' });
  if (data) jumlah.value = count;
};

// Filter data berdasarkan pencarian
const filteredVisitors = computed(() => {
  if (!keyword.value) return visitors.value;
  return visitors.value.filter(visitor =>
    visitor.nama.toLowerCase().includes(keyword.value.toLowerCase())
  );
});

// Load data ketika halaman dimuat
onMounted(() => {
  getsiswa();
  getjumlah();
});
</script>

<style scoped>
i {
  color: black;
  font-size: 2em;
}
</style>

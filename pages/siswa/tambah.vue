<template>
  <div class="container-fluid">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    <div class="row">
      <div class="col-lg-12">
        <!-- Title Page -->
        <h2 class="text-center my-4" id="page-title">RIWAYAT</h2>

        <!-- Link Kembali -->
        <nuxt-link to="/halaman2">
          <i class="bi bi-arrow-left" aria-label="Kembali ke halaman sebelumnya"></i>
        </nuxt-link>

        <!-- Form Pencarian -->
        <div class="my-3">
          <form @submit.prevent="getsiswa" aria-labelledby="search-label">
            <label for="searchInput" id="search-label" class="visually-hidden">Pencarian Siswa</label>
            <input 
              v-model="keyword" 
              type="search" 
              id="searchInput" 
              name="searchInput" 
              class="form-control rounded-5" 
              placeholder="Cari nama"
              aria-describedby="search-helper"
              autocomplete="off"
            />
            <div id="search-helper" class="form-text">Masukkan nama siswa untuk pencarian</div>
          </form>
        </div>

        <!-- Pesan ketika tidak ada data ditemukan -->
        <div v-if="filteredVisitors.length === 0" class="text-center text-danger" role="alert" aria-live="assertive">
          Tidak ada nama yang cocok dengan pencarian.
        </div>

        <!-- Tabel Riwayat -->
        <div class="table-responsive">
          <table class="table table-bordered" aria-labelledby="table-label">
            <caption id="table-label">Tabel Riwayat Kehadiran Siswa</caption>
            <thead>
              <tr>
                <th scope="col">No</th>
                <th scope="col">NAMA</th>
                <th scope="col">KETERANGAN</th>
                <th scope="col">WAKTU</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(visitor, i) in filteredVisitors" :key="visitor.id">
                <td>{{ i + 1 }}.</td>
                <td>{{ visitor.nama?.nama || 'Nama tidak ada' }}</td>
                <td>{{ visitor.keterangan?.keterangan || 'Tidak ada keterangan' }}</td>
                <td>{{ visitor.created_at }}</td>
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
const supabase = useSupabaseClient();  // Menggunakan Supabase Client
const keyword = ref('');               // Ref untuk input pencarian
const visitors = ref([]);              // Ref untuk data riwayat
const jumlah = ref(0);                 // Ref untuk jumlah total siswa

// Fungsi untuk mengambil data riwayat
const getsiswa = async () => {
  try {
    const { data, error } = await supabase.from('riwayat')
      .select('*, nama(*), keterangan(*)')  // Mengambil data yang relevan
      .order('id', { ascending: false });  // Mengurutkan berdasarkan ID terbaru

    if (error) {
      console.error('Error fetching riwayat data:', error);
      return;
    }
    if (data) {
      visitors.value = data;  // Menyimpan data riwayat ke dalam visitors
      getjumlah();             // Memperbarui jumlah siswa
    }
  } catch (error) {
    console.error('Error fetching riwayat data:', error);
  }
};

// Fungsi untuk mengambil jumlah siswa
const getjumlah = async () => {
  try {
    const { count, error } = await supabase.from('siswa').select('*', { count: 'exact' });
    if (error) {
      console.error('Error fetching student count:', error);
      return;
    }
    if (count) jumlah.value = count;
  } catch (error) {
    console.error('Error fetching student count:', error);
  }
};

// Filter berdasarkan keyword pencarian
const filteredVisitors = computed(() => {
  if (!keyword.value) return visitors.value;
  return visitors.value.filter(visitor =>
    visitor.nama?.nama.toLowerCase().includes(keyword.value.toLowerCase())
  );
});

// Memuat data saat komponen pertama kali dimuat
onMounted(() => {
  getsiswa();
});
</script>

<style scoped>
/* Untuk menyembunyikan label pencarian tetapi tetap dapat diakses oleh pembaca layar */
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  border: 0;
  clip: rect(0, 0, 0, 0);
  overflow: hidden;
}

/* Styling untuk ikon kembali */
i {
  color: black;
  font-size: 2em;
}

/* Styling untuk tabel agar lebih responsif */
.table-responsive {
  max-width: 100%;
  overflow-x: auto; /* Tambahkan scroll horizontal jika tabel terlalu lebar */
}

/* Pengaturan ukuran font dan padding untuk tabel */
.table th, .table td {
  text-align: center;
  vertical-align: middle;
  padding: 10px; /* Padding sel untuk kenyamanan membaca */
  word-wrap: break-word; /* Memastikan teks panjang tetap wrap di dalam kolom */
}

/* Membuat ukuran font lebih kecil pada perangkat mobile */
@media (max-width: 576px) {
  .table th, .table td {
    font-size: 0.875rem; /* Ukuran font lebih kecil pada perangkat mobile */
    padding: 8px; /* Padding lebih kecil untuk tampilan mobile */
  }
}
</style>

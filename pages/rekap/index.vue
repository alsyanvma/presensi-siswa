<template>
  <div class="container-fluid">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">REKAP</h2>
        <nuxt-link to="/">
          <i class="bi bi-arrow-left"></i>
        </nuxt-link>
        <div class="my-3">
          <table class="table table-bordered">
            <thead>
              <tr>
                <th scope="col">WAKTU</th>
                <th scope="col">JUMLAH IZIN</th>
                <th scope="col">JUMLAH SAKIT</th>
                <th scope="col">JUMLAH ALFA</th>
              </tr>
            </thead>
            <tbody>
              <!-- Looping data rekap -->
              <tr v-for="(rekap, index) in rekapData" :key="index">
                <td>{{ formatTanggal(rekap.waktu) }}</td>
                <td>{{ rekap.jumlahIzin.toLocaleString("id-ID") }}</td>
                <td>{{ rekap.jumlahSakit.toLocaleString("id-ID") }}</td>
                <td>{{ rekap.jumlahAlfa.toLocaleString("id-ID") }}</td>
              </tr>
              <!-- Pesan jika tidak ada data -->
              <tr v-if="rekapData.length === 0">
                <td colspan="4" class="text-center">Tidak ada data tersedia</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient(); // Inisialisasi Supabase Client
const rekapData = ref([]); // Variabel untuk menyimpan data rekap

// Fungsi untuk memformat tanggal menjadi format yang mudah dibaca
const formatTanggal = (tanggal) => {
  const options = { year: "numeric", month: "long", day: "numeric" };
  return new Date(tanggal).toLocaleDateString("id-ID", options);
};

// Fungsi untuk menghitung dan mengambil data rekap dari tabel riwayat
const fetchRekapData = async () => {
  try {
    // Mengambil data dari tabel riwayat, termasuk keterangan dan created_at
    const { data: pengunjungData, error } = await supabase
      .from("riwayat")
      .select("keterangan (keterangan), created_at");

    if (error) throw error;

    console.log("Data Presensi:", pengunjungData); // Log data untuk debugging

    // Objek untuk menyimpan hasil rekap berdasarkan created_at
    const rekap = {};

    // Looping untuk menghitung jumlah izin, sakit, dan alfa berdasarkan created_at
    pengunjungData.forEach(visitor => {
      const key = visitor.created_at; // Menggunakan created_at sebagai kunci utama

      // Jika belum ada data untuk tanggal tersebut, buat entri baru
      if (!rekap[key]) {
        rekap[key] = { waktu: key, jumlahIzin: 0, jumlahSakit: 0, jumlahAlfa: 0 };
      }

      // Memastikan keterangan ada, jika tidak ada beri nilai default 'Tidak Diketahui'
      const keterangan = visitor.keterangan?.keterangan || 'Tidak Diketahui';
      console.log(`Keterangan pada ${key}:`, keterangan); // Log keterangan untuk debugging

      // Switch case untuk mengelompokkan jumlah izin, sakit, dan alfa
      switch (keterangan) {
        case 'Izin':
          rekap[key].jumlahIzin++;
          break;
        case 'Sakit':
          rekap[key].jumlahSakit++;
          break;
        case 'Alfa':
          rekap[key].jumlahAlfa++;
          break;
      }
    });

    // Mengubah objek rekap menjadi array dan mengurutkannya berdasarkan created_at (terbaru ke terlama)
    rekapData.value = Object.values(rekap).sort((a, b) => new Date(b.waktu) - new Date(a.waktu));
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};

// Memanggil fungsi fetchRekapData ketika komponen di-mount
onMounted(() => {
  fetchRekapData();
});
</script>

<style scoped>
i {
  color: black;
  font-size: 2em;
}
.table {
  margin-top: 20px;
}
.table th, .table td {
  text-align: center;
  vertical-align: middle;
}
</style>

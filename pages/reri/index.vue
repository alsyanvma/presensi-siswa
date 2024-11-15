<template>
  <div class="container-fluid">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    <div class="row">
      <div class="col-lg-12">
        <!-- Menambahkan id untuk header dan label untuk aksesibilitas -->
        <h2 class="text-center my-4" id="rekapHarianTitle">REKAP HARIAN</h2>
        <nuxt-link to="/halaman1" aria-label="Kembali ke halaman sebelumnya">
          <i class="bi bi-arrow-left"></i>
        </nuxt-link>

        <!-- Form pencarian dengan label yang jelas untuk aksesibilitas -->
        <div class="my-3 d-flex justify-content-center">
          <label for="searchDate" class="visually-hidden">Pilih Tanggal</label>
          <input
            id="searchDate"
            type="date"
            v-model="searchDate"
            class="form-control w-50"
            aria-describedby="searchDateDescription"
            aria-label="Pilih tanggal untuk filter data rekap harian"
          />
          <span id="searchDateDescription" class="visually-hidden">
            Pilih tanggal untuk filter data rekap harian
          </span>
        </div>

        <!-- Tabel dengan aksesibilitas lebih baik -->
        <div class="my-3">
          <div class="table-responsive">
            <table class="table table-bordered" aria-labelledby="rekapHarianTitle">
              <thead>
                <tr>
                  <th scope="col" aria-label="Waktu Kehadiran">WAKTU</th>
                  <th scope="col" aria-label="Jumlah Izin">JUMLAH IZIN</th>
                  <th scope="col" aria-label="Jumlah Sakit">JUMLAH SAKIT</th>
                  <th scope="col" aria-label="Jumlah Alpa">JUMLAH ALPA</th>
                  <th scope="col" aria-label="Jumlah Dispensasi">JUMLAH DISPEN</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(rekap, index) in filteredRekapData" :key="index">
                  <td>{{ formatTanggal(rekap.waktu) }}</td>
                  <td>{{ rekap.jumlahIzin.toLocaleString("id-ID") }}</td>
                  <td>{{ rekap.jumlahSakit.toLocaleString("id-ID") }}</td>
                  <td>{{ rekap.jumlahAlfa.toLocaleString("id-ID") }}</td>
                  <td>{{ rekap.jumlahDispen.toLocaleString("id-ID") }}</td>
                </tr>
                <tr v-if="filteredRekapData.length === 0">
                  <td colspan="5" class="text-center">Tidak ada data tersedia</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
const supabase = useSupabaseClient();
const rekapData = ref([]);
const searchDate = ref("");

// Format tanggal untuk tampilan
const formatTanggal = (tanggal) => {
  const options = { year: "numeric", month: "long", day: "numeric" };
  return new Date(tanggal).toLocaleDateString("id-ID", options);
};

// Ambil data rekap dari Supabase
const fetchRekapData = async () => {
  try {
    const { data: siswaData, error } = await supabase
      .from("riwayat")
      .select("keterangan (keterangan), created_at");

    if (error) throw error;

    console.log("Fetched Data:", siswaData);

    const rekap = {};
    siswaData.forEach((visitor) => {
      const key = visitor.created_at;

      if (!rekap[key]) {
        rekap[key] = { waktu: key, jumlahIzin: 0, jumlahSakit: 0, jumlahAlfa: 0, jumlahDispen: 0 };
      }

      const keterangan = (visitor.keterangan?.keterangan || '').trim().toLowerCase();
      console.log(`Processing Keterangan: ${keterangan} on ${key}`);

      switch (keterangan) {
        case 'izin':
          rekap[key].jumlahIzin++;
          break;
        case 'sakit':
          rekap[key].jumlahSakit++;
          break;
        case 'alpa':
          rekap[key].jumlahAlfa++;
          break;
        case 'dispen':
          rekap[key].jumlahDispen++;
          break;
        default:
          console.log(`Unrecognized keterangan: ${keterangan}`);
      }
    });

    console.log("Processed Rekap Data:", rekap);
    rekapData.value = Object.values(rekap).sort((a, b) => new Date(b.waktu) - new Date(a.waktu));
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};

// Filter data berdasarkan tanggal
const filteredRekapData = computed(() => {
  if (!searchDate.value) {
    return rekapData.value;
  }

  return rekapData.value.filter((rekap) => {
    const formattedDate = new Date(rekap.waktu).toISOString().split("T")[0];
    return formattedDate === searchDate.value;
  });
});

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
  table-layout: fixed; /* Agar kolom tabel tidak meluber */
  width: 100%; /* Pastikan tabel memenuhi lebar kontainer */
}

.table th, .table td {
  text-align: center;
  vertical-align: middle;
  padding: 10px; /* Padding sel untuk kenyamanan membaca */
  word-wrap: break-word; /* Memastikan teks panjang tetap wrap di dalam kolom */
}

.table-responsive {
  max-width: 100%; /* Agar tabel tidak melebihi lebar kontainer */
  overflow-x: auto; /* Tambahkan scroll horizontal pada perangkat kecil */
}

@media (max-width: 576px) {
  .table th, .table td {
    font-size: 0.875rem; /* Ukuran font lebih kecil pada perangkat mobile */
    padding: 8px; /* Padding lebih kecil untuk tampilan mobile */
  }
}

/* Menambahkan fokus yang jelas pada elemen interaktif */
input:focus,
button:focus {
  outline: 2px solid #4d90fe; /* Mengganti outline default dengan warna yang lebih mencolok */
  outline-offset: 2px;
}
</style>

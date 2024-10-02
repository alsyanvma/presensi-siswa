<template>
    <div class="container-fluid">
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
        <div class="row">
            <div class="col-lg-12">
                <h2 class="text-center my-4">RIWAYAT</h2>
                <nuxt-link to="/">
                <i class="bi bi-arrow-left"></i>
                </nuxt-link>
                <div class="my-3">
                    <form @submit.prevent="getsiswa">
                        <input v-model="keyword" type="search" class="form-control rounded-5" placeholder="Cari">
                    </form>
                    <div class="my-3 text-muted"> menampilkan {{ visitors.length }} dari {{ jumlah}} </div>
                    <table class="table">
                        <thead>
                        <tr>
                        <th scope="col">NAMA</th>
                        <th scope="col">KETERANGAN</th>
                        <th scope="col">WAKTU</th>
                        </tr>
                    </thead>
                        <tbody>
                            <tr v-for="(visitor,i) in visitors" :key="i">
                                <td>{{ i+1 }}.</td>
                                <td>{{ visitor.nama}}</td>
                                <td>{{ visitor.keanggotaan.nama}}</td>
                                <td>{{ visitor.tanggal }},{{ visitor.waktu}}</td>
                                <td>{{ visitor.keterangan.nama}}</td>
                            </tr>
                    </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const keyword = ref('')
const visitors = ref([])
const jumlah = ref ([])

const getpengunjung = async () => {
    const {data, error} = await supabase.from('pengunjung').select(`*, keanggotaan(*), keterangan(*)`)
        .ilike('nama', `%${keyword.value}%`)
        .order(`id`, {ascending:false})
    if(data) visitors.value = data
}

const totalPengunjung = async () => {
    const { data, count } = await supabase.from('pengunjung')
    .select("*", {count: 'exact'})
    if (data) jumlah.value = count
}

onMounted(()=> {
    getpengunjung()
    totalPengunjung()
})

</script>

<style scoped>
i{
    color: black;
    font-size: 2em;
}
</style>
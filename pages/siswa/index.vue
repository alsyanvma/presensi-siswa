<template>
    <div class="container-fluid">
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
        <div class="row">
            <div class="col-lg-12">
                <h2 class="text-center my-4">RIWAYAT KUNJUNGAN</h2>
                <nuxt-link to="/">
                <i class="bi bi-arrow-left"></i>
                </nuxt-link>
                <div class="my-3">
                    <form @submit.prevent="getsiswa">
                        <input v-model="keyword" type="search" class="form-control rounded-5" placeholder="Cari">
                    </form>
                    <div class="my-3 text-muted"> menampilkan {{ visitors.length }} dari {{ jumlah }} siswa </div>
                    <table class="table table-bordered">
                        <thead>
                            <tr>
                                <td scope="col">No</td>
                                <td scope="col">NAMA</td>
                                <td scope="col">KETERANGAN</td>
                                <td scope="col">WAKTU</td>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(visitor,i) in visitors" :key="i">
                                <td>{{ i+1 }}.</td>
                                <td>{{ visitor.nama.nama }}</td>
                                <td>{{ visitor.keterangan.keterangan }}</td>
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
const supabase = useSupabaseClient()
const keyword = ref('')
const visitors = ref([])
const jumlah = ref ([])

const getsiswa = async () => {
    const {data, error} = await supabase.from('riwayat').select(`*, nama(*), keterangan(*)`)
        .ilike('nama.nama', `%${keyword.value}%`)
    if(data) visitors.value = data
}



onMounted(()=> {
    getsiswa()

})

</script>

<style scoped>
i{
    color: black;
    font-size: 2em;
}
</style>
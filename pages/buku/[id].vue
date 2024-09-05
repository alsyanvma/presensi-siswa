<template>
    <div class="container-fluid">
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
        <nuxt-link to="/buku">
        <i class="bi bi-arrow-left "></i>
        </nuxt-link>
        <h2 class="text-start my-4">{{ buku.judul}}</h2>
        <div class="row">
            <div class="col-md-3">
                <img :src="buku.cover" class="cover" alt="cover buku ">
            </div>
            <div class="col-md-8">
                <ul class="list-group list-group-flush">
                    <li class="list-group-item">Penulis : {{ buku.penulis}}</li>
                    <li class="list-group-item">Penerbit : {{ buku.penerbit}}</li>
                    <li class="list-group-item">Tahun_terbit : {{ buku.tahun_terbit}}</li>
                    <li class="list-group-item">{{ buku.deskripsi}}</li>
                    <li class="list-group-item">
                        <span v-if="buku.kategori">Kategori : {{ buku.kategori.nama }}</span>
                        <span v-else>loading...</span>
                    </li>
                </ul>
                <nuxt-link to="/">
                    <button type="submit" class="btn btn-lg rounded-5 px-5 bg-success text-white" style="float: right;">Pinjam</button>
                </nuxt-link>
            </div>
        </div>
    </div>
</template>

<style scoped>
.btn.bg-success {
    background-color: rgb(156, 109, 201) !important;
}

i{
    color: black;
    font-size: 2em;
}
</style>

<script setup>
const supabase = useSupabaseClient()

const route = useRoute()
const buku = ref([])

const getBooksById = async () => {
    const { data, error } = await supabase.from('buku').select(`*, kategori(*)`)
    .eq('id', route.params.id)
    if(data) buku.value = data[0]
}

onMounted(() => {
    getBooksById()
})

</script>


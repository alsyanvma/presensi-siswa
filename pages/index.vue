<template>
    <div class="container-fluid">
        <div class="row my-5">
            <div class="col-lg-6">
                <nuxt-link to="/pengunjung/tambah">
                    <div class="card bg-success rounded-5">
                        <div class="card-body">
                            <h5>Isi Kehadiran</h5>
                        </div>
                    </div>
                </nuxt-link>
            </div>
            <div class="col-lg-6">
                <nuxt-link to="/buku">
                    <div class="card bg-success rounded-5">
                        <div clas="card-body">
                            <h1>Rekap Data</h1>
                        </div>
                    </div>
                </nuxt-link>
            </div>
        </div>
        <div class="row my-5">
            <div class="col-lg-6">
                <nuxt-link to ="/pengunjung">
                <div class="card bg-success rounded-5">
                    <div class="card-body">
                        <h2 class="font"> {{ jumlahpengunjung}} Tambah Siswa </h2>                    </div>
                </div>
                </nuxt-link>
            </div>
        
            <div class="col-lg-6">
                <nuxt-link to="/buku">
                <div class="card bg-success rounded-5">
                    <div class="card-body">
                        <h6 class="font"> {{jumlahbuku}} Data Siswa </h6>
                    </div>
                </div>
                </nuxt-link>
            </div>
        </div>
    </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const jumlahpengunjung = ref (0);
const jumlahbuku = ref (0);


async function ambiljumlahpengunjung() {
    const { data,error, count} = await supabase
    .from("pengunjung")
    .select("*", {count: 'exact'});
    if (count) jumlahpengunjung.value = count;
}
async function ambiljumlahbuku() {
    const { data,error, count} = await supabase
    .from("buku")
    .select("*", {count: 'exact'});
    if (count) jumlahbuku.value = count;
}


onMounted(() => {
    ambiljumlahpengunjung();
    ambiljumlahbuku();
})
</script>





<style scoped>

.card {
    height: 200px;
    margin-right: 27px;
    box-shadow: 1px 1px 10px #ffffff;
}

h1 {
    color: rgb(255, 255, 255);
    font-size: 30px;
    text-align: center;
    margin-top: 65px;

}
    

h2 {
    color: rgb(255, 255, 255);
    font-size: 30px;
    text-align: center;
    margin: 70px;
}



h5{
    color: rgb(255, 255, 255);
    justify-content: center;
    font-size: 30px;
    text-align: center;
    margin-top: 50px;
}


h6 {
    color: rgb(255, 255, 255);
    font-size: 30px;
    text-align: center;
    margin: 70px;
}

</style>
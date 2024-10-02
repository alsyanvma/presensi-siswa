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
                  <form @submit.prevent="getsiswa">
                  </form>
                  <table class="table table-bordered">
                      <thead>
                          <tr>
                              <td scope="col">WAKTU</td>
                              <td scope="col">SAKIT</td>
                              <td scope="col">IZIN</td>
                              <td scope="col">ALFA</td>
                          </tr>
                      </thead>
                      <tbody>
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
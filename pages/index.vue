<template>
    <div class="container-fluid">
        <div class="row my-5">
            <div class="col-lg-6">
                <nuxt-link to="/siswa/tambah">
                    <div class="card bg-success rounded-5">
                        <div class="card-body">
                            <h5>Isi Kehadiran</h5>
                        </div>
                    </div>
                </nuxt-link>
            </div>
            <div class="col-lg-6">
                <nuxt-link to="/siswa">
                    <div class="card bg-success rounded-5">
                        <div clas="card-body">
                            <h1>Riwayat</h1>
                        </div>
                    </div>
                </nuxt-link>
            </div>
        </div>
        <div class="row my-5">
            <div class="col-lg-6">
                <nuxt-link to ="/rekap">
                <div class="card bg-success rounded-5">
                    <div class="card-body">
                        <h2 class="font">Rekap </h2>                    
                    </div>
                </div>
                </nuxt-link>
            </div>
        
            <div class="col-lg-6">
                <nuxt-link to="/data">
                <div class="card bg-success rounded-5">
                    <div class="card-body">
                        <h6 class="font">Data Siswa </h6>
                    </div>
                </div>
                </nuxt-link>
            </div>
        </div>
    </div>
</template>







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
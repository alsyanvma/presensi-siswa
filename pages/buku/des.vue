<template>
    <div class="contaner-fluid">
        <div class="row">
            <div class="col-lg-12">
                <div class="row">
                    <div class="col-lg-2">
                        <div class="card mb-3">
                            <div class="card-body">
                                <img src="~/assets/img/cover1.jpg" class="cover" alt="cover 1" width="200px">
                            </div>
                        </div>
                    </div>
                    <div class="col-lg-6">
                        <h1>Mariposa</h1>
                        <p>Pengarang    : Hidayatul Fajriyah</p>
                        <p>Negara       : Indonesia</p>
                        <p>Bahasa       : Indonesia</p>
                        <p>Penerbit     : Coconut books</p>
                        <p>Tahun Terbit : 2018</p>
                        <p>Halaman      : 496 Halaman</p>
                        <p>Deskripsi   : Mengisahkan seorang gadis cantik bernama Natasha Kay Loovi atau kerap disapa Acha yang memperjuangkan cintanya terhadap seorang laki-laki berhati beku dan super dingin–bagaikan es–dengan kehidupannya yang serba monoton, bernama Iqbal. </p>
                    </div>
                    <nuxt-link to="../"><button type="submit" class="btn btn-lg rounded-5 px-5 bg-success text-white" style="float: right;">PINJAM</button></nuxt-link>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>

.cover1{
    height: 20px;
    width: 20px;
}

.btn.bg-success {
    background-color: rgb(164, 127, 199) !important;
}

.bg-purple{
    color: 5361AF;
}

.card.mb-3{
    margin-top: 30px;
    width: 240px;
    height: 60x;
    margin-left: 20px;
}

h1 {
    margin-top: 30px;
    margin-left: 80px;
    margin-bottom: 1px;
    
    
}

p {
    margin-left: 80px;
    margin-bottom: 7px;
}
</style>
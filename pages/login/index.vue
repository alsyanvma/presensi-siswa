<script setup>
const supabase = useSupabaseClient();
const email = ref("");
const password = ref("");
const errorMessage = ref("");

const Login = async () => {
  if (!email.value || !password.value) {
    errorMessage.value = "wajib diisi!";
    return;
  }

  // Proses login
  const { data, error } = await supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  });

  if (error) {
    errorMessage.value = error.message;  
  }

  if (data) {
    navigateTo("/halaman1");
    
  }
};
</script>
<template>
    <div class="container-fluid mt-5">
      <div class="row justify-content-center">
        <div class="col-md-6">
          <h2>Login</h2>
          <form @submit.prevent="Login">
            <div class="mb-3">
              <label for="email" class="form-label">Email address</label>
              <input 
                v-model="email" 
                type="email" 
                class="form-control" 
                
              />
            </div>
            <div class="mb-3">
              <label for="password" class="form-label">Password</label>
              <input 
                v-model="password" 
                type="password" 
                class="form-control" 
              />
            </div>
            <div>
              <button>Login</button>
            </div>
            <!-- <button 
              type="submit" 
              class="btn btn-primary" 
              :disabled="loading"
            >
              <span v-if="loading">Loading...</span>
              <span v-else>Login</span>
            </button>
            <div v-if="error" class="alert alert-danger mt-3">
              {{ error }}
            </div> -->
          </form>     
        </div>
      </div>
    </div>
  </template>

<style scoped>

</style>
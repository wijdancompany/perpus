<template>
  <div>
    
    <div class="my-3">
      <form @submit.prevent="getData">
        <input 
          v-model="keyword"
          type="search" 
          class="form-control form-control-lg rounded-pill" 
          placeholder="Mau baca apa hari ini...">
      </form>
    </div>

    <div class="row">
      <div v-for="book in books" :key="book.id" class="col-3">
        <div class="card">
          <div class="card-header">
            <img :src="book.cover" alt="cover" class="cover">
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const books = ref([])
const keyword = ref('')

onMounted(() => getData())

async function getData() {
  let { data, error } = await supabase
    .from('buku')
    .select(`
      id, judul, penulis, penerbit, cover,
      kategori(nama), rak(kode)
    `)
    .ilike('judul', `%${keyword.value}%`)
  if(data) books.value = data
  if(error) throw error
}
</script>

<style scoped>
.cover {
  width: 100%
}
</style>
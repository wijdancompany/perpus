<template>
  <div>
    
    <div class="my-3">
      <input type="search" class="form-control form-control-lg rounded-pill" placeholder="Mau baca apa hari ini...">
    </div>

    <table class="table">
      <thead>
        <tr>
          <td>COVER</td>
          <td>JUDUL</td>
          <td>PENULIS</td>
          <td>PENERBIT</td>
          <td>KATEGORI</td>
          <td>RAK</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="book in books" :key="book.id">
          <td><img :src="book.cover" alt="cover" class="cover"></td>
          <td>{{ book.judul }}</td>
          <td>{{ book.penulis }}</td>
          <td>{{ book.penerbit }}</td>
          <td>{{ book.kategori.nama }}</td>
          <td>{{ book.rak.kode }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const books = ref([])

onMounted(() => getData())

async function getData() {
  let { data, error } = await supabase
    .from('buku')
    .select(`
      id, judul, penulis, penerbit, cover,
      kategori(nama), rak(kode)
    `)
  if(data) books.value = data
  if(error) throw error
}
</script>

<style scoped>
.cover {
  width: 90px
}
</style>
<script setup>
  import {ref} from 'vue'

  // Variables
  const newItem = ref("")
  const datas = ref([])
  const setError = ref(false)
  const error = ref("Veillez remplir convenablement le champ de saisi !")
  const dataUpdate = ref()

  //Functions
  const onSubmit = () => {
    setError.value = false
    if(dataUpdate.value){
      dataUpdate.value.content = newItem.value
      newItem.value = ""
      dataUpdate.value = null
    }else{
      if(newItem.value){
        const item = {
          content: newItem.value,
          id: Date.now(),
          status: false
        }
        datas.value.push(item)
        newItem.value = ""
      }else{
        setError.value = true
      }
    }
  }

  const updateItem = (itemUpdate) => {
    newItem.value = itemUpdate.content
    dataUpdate.value = itemUpdate
  }

  const deleteItem = (id) => {
    if(id){
      datas.value = datas.value.filter(data => id !== data.id)
    }
  }
</script>

<template>
  <div class="max-w-[500px] mx-auto p-5">
    <form @submit.prevent="onSubmit" class="mb-5">
      <div>
        <label for="newItem" class="text-xl font-bold">Note</label><br>
        <div class="flex justify-between bg-stone-200 p-2 rounded-xl">
          <input type="text" name="newItem" v-model="newItem" class="w-full outline-none p-2">
          <button class="bg-blue-500 px-5 py-2 rounded-xl text-white font-bold cursor-pointer flex align-middle">Enregistrer</button>
        </div>
        <p v-if="setError" class="text-red-500 p-2 ps-0">{{error}}</p>
      </div>
    </form>

    <div v-if="datas.value">
      <ul>
        <li v-for="data in datas" class="flex align-middle justify-between bg-stone-200 p-2 my-2 rounded-sm">
          <p >{{ data.content }}</p>
          <div class="grid grid-cols-2 gap-1">
            <button @click="updateItem(data)" class="bg-blue-500 rounded-sm text-white cursor-pointer min-w-[100px] h-[35px]">Update</button>
          <button @click="deleteItem(data.id)" class="bg-red-500 rounded-sm text-white cursor-pointer min-w-[100px] h-[35px]">Delete</button>
          </div>
        </li>
      </ul>
    </div>
    <div v-else>
      <h3 class="text-center text-xl">Aucune tache audjourd'hui</h3>
    </div>
  </div>
</template>

<style scoped></style>

<script setup>
  import {ref} from 'vue'
  import Item from '@/components/Item.vue'
  import ListItem from '@/components/ListItem.vue'

  // Variables
  const newItem = ref("")
  const datas = ref([])
  const dataFilter = ref([])
  const setError = ref(false)
  const error = ref("Veillez remplir convenablement le champ de saisi !")
  const dataUpdate = ref()
  const isActiveAll = ref(true)
  const isActiveTermine = ref(false)
  const isActiveRecent = ref(false)
  dataFilter.value = datas.value
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
    dataFilter.value = datas.value
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

  const endTask = (item) => {
    console.log('Je suis bien atteint')
    if(item.content){
      item.status = true
    }
  }

  const showRecentTask = () => {
    isActiveAll.value = false
    isActiveTermine.value = false
    isActiveRecent.value = true
    dataFilter.value = datas.value.toSorted((a,b) =>  b.id - a.id).slice(0,2)
  }

  const showAll = () => {
    isActiveAll.value = true
    isActiveTermine.value = false
    isActiveRecent.value = false
    dataFilter.value = datas.value.toSorted((a,b) => a.status > b.status)
  }

  const showTaskEnd = () => {
    isActiveAll.value = false
    isActiveTermine.value = true
    isActiveRecent.value = false
    dataFilter.value = datas.value.filter(d => d.status === true)
  }

</script>

<template>
  <div class="max-w-[800px] mx-auto p-5">
    <form @submit.prevent="onSubmit" class="mb-5">
      <div>
        <label for="newItem" class="text-xl font-bold">Tache</label><br>
        <div class="flex justify-between bg-stone-200 p-2 rounded-xl">
          <input type="text" name="newItem" v-model="newItem" class="w-full outline-none p-2">
          <button class="bg-blue-500 px-5 py-2 rounded-xl text-white font-bold cursor-pointer flex align-middle">Enregistrer</button>
        </div>
        <p v-if="setError" class="text-red-500 p-2 ps-0">{{error}}</p>
      </div>
    </form>

    <div v-if="datas.length > 0">
      <ListItem>
        <div>
          <ul class="flex flex-column my-5">
            <li :class="{active : isActiveAll }" class="cursor-pointer hover:bg-stone-100 p-2 min-w-[100px]" @click="showAll">Tous</li>
            <li :class="{active : isActiveRecent }" class="cursor-pointer hover:bg-stone-100 border-s-1 border-stone-100 p-2 min-w-[100px]" @click="showRecentTask">Recent(s)</li>
            <li :class="{active : isActiveTermine }" class="cursor-pointer hover:bg-stone-100 border-s-1 border-stone-100 p-2 min-w-[100px]" @click="showTaskEnd">Terminee(s)</li>
          </ul>
        </div>
        <Item v-if="dataFilter.length > 0" v-for="data in dataFilter" :data="data" @update-item="updateItem" @delete-item="deleteItem" @end-task="endTask" />
          <h3 v-else class="text-center text-xl">Aucune tache terminees</h3>
      </ListItem>
    </div>
    <div v-else>
      <h3 class="text-center text-xl">Aucune tache audjourd'hui</h3>
    </div>
  </div>
</template>

<style scoped>
  .active{
    background-color: var(--color-stone-200);
  }
</style>

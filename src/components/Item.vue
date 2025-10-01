<template>
<li class="flex align-middle justify-between bg-stone-200 p-2 my-2 rounded-sm">
    <p >{{ data.content }}</p>
    <p v-if="data.status" class="text-white bg-green-500 p-2 py-1 rounded-3xl">Task complete</p>
    <div v-else class="grid grid-cols-3 gap-1">
    	<button @click="$emit('endTask', data)" class="bg-green-500 rounded-sm text-white cursor-pointer min-w-[100px] h-[35px]">End task</button>
        <button @click="$emit('updateItem', data)" class="bg-blue-500 rounded-sm text-white cursor-pointer min-w-[100px] h-[35px]">Update</button>
        <button @click="$emit('deleteItem', data.id)" class="bg-red-500 rounded-sm text-white cursor-pointer min-w-[80px] h-[35px]">Delete</button>
    </div>
</li>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

defineProps({
	data : {
		type: Object,
		require: true
	}
});

defineEmits(['updateItem', 'deleteItem', 'endTask']);
</script>
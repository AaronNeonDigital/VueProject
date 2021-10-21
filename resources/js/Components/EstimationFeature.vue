<template>
    
            <button 
                @click="toggleFeature(index)"
                class="text-white py-4 px-6 block hover:text-blue-100 focus:outline-none text-blue-200 border-b-2 font-medium border-blue-500"
            >
                {{ index }}
            </button>
    <!---
    <div @click="toggleFeature(index)"
        v-bind:class="{ active: features[index].isActive, 'text-green-600': 'text-red-600' }">
        <span class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4 text-left text-blueGray-700 ">
            {{ index }}
        </span>
    </div>--->
</template>

<script>
export default {
    props: ['index', 'item', 'toggleFeature', 'features']
}
</script>
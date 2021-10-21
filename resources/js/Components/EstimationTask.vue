<template>
    <div v-if="features[index].show">
        <ul>
            <li 
                v-for="(task, i) in features[index].tasks" :key="i"
                class="py-2 px-4 text-white"
                >
                <button v-if="!features[index].tasks[i].inUse"
                    @click.prevent="toggleUse(index, i)"
                    class="bg-red-600 rounded p-1 text-white hover:bg-red-700 hover:text-red-200 z-50">
                    
                    <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                    
                </button>
                <button v-else
                    @click.prevent="toggleUse(index, i)"
                    class="bg-green-600 rounded p-1 text-white hover:bg-green-700 hover:text-green-200 z-50">
                    <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20" stroke="currentColor" aria-hidden="true">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7.629,14.566c0.125,0.125,0.291,0.188,0.456,0.188c0.164,0,0.329-0.062,0.456-0.188l8.219-8.221c0.252-0.252,0.252-0.659,0-0.911c-0.252-0.252-0.659-0.252-0.911,0l-7.764,7.763L4.152,9.267c-0.252-0.251-0.66-0.251-0.911,0c-0.252,0.252-0.252,0.66,0,0.911L7.629,14.566z"></path>
                    </svg>
                </button>
                {{ i }} 
                <span class="float-right mx-3 rounded py-1 px-3" v-bind:class = "findColorForRisk(standardDeviation([task.optimistic, task.pessimistic, task.likely]))">
                    Risk: {{ standardDeviation([task.optimistic, task.pessimistic, task.likely]) }}%
                </span>
                <span class="float-right mx-3">Budget: {{ hoursTotal(task.optimistic, task.pessimistic, task.likely) }}</span>
                <EstimationRow  v-if="features[index].tasks[i].inUse"
                    :features = "features"
                    :featureIndex = "index"
                    :taskIndex = "i"
                    :task = "task"
                />
            </li>
        </ul>
    </div>
</template>

<script>

import EstimationRow from '@/Components/EstimationRow.vue';

export default {
    components: {
        EstimationRow
    },
    props: ['index', 'features', 'items', 'hoursTotal', 'standardDeviation'],
    methods: {
        
        toggleUse(index, key){
            return this.features[index].tasks[key].inUse = !this.features[index].tasks[key].inUse
        },
        findColorForRisk(value) {
            return value > 250 ? 'text-red-100 bg-red-800' : (
                value > 100 ? 'text-yellow-100 bg-yellow-800' : 'text-green-100 bg-green-800'
                );
        }
    }
}
</script>
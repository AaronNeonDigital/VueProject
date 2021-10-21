<template>
    <span>
        <!--Value fields--->
        <div class="w-full grid grid-cols-3">
            <div class="flex flex-row h-10">
                <span class="text-sm border border-blue-600 rounded-l px-4 py-2 bg-blue-500 text-white whitespace-no-wrap">Optimistic:</span>
                <button
                    @click="decrementNumber(featureIndex, taskIndex, 'optimistic')" 
                    class="bg-red-300 text-red-600 hover:text-red-700 hover:bg-red-400 h-full w-10 cursor-pointer outline-none">
                <span class="m-auto text-2xl font-thin">−</span>
                </button>
                <input type="number" class="border border-blue-600 outline-none focus:outline-none text-center w-24 bg-gray-300 font-semibold text-md hover:text-black focus:text-black  md:text-basecursor-default flex items-center text-gray-700  outline-none" 
                    name="custom-input-number" 
                    v-model="task.optimistic"/>
                <button
                    @click="incrementNumber(featureIndex, taskIndex, 'optimistic')"
                    class="bg-green-300 text-green-600 hover:text-green-700 hover:bg-green-400 h-full w-10 rounded-r cursor-pointer">
                    <span class="m-auto text-2xl font-thin">+</span>
                </button>
            </div>
            <div class="flex flex-row h-10">
                <span class="text-sm border border-blue-600 rounded-l px-4 py-2 bg-blue-500 text-white whitespace-no-wrap">Likely:</span>
                <button
                    @click="decrementNumber(featureIndex, taskIndex, 'likely')" 
                    class="bg-red-300 text-red-600 hover:text-red-700 hover:bg-red-400 h-full w-10 cursor-pointer outline-none">
                <span class="m-auto text-2xl font-thin">−</span>
                </button>
                <input type="number" class="outline-none focus:outline-none text-center w-24 bg-gray-300 font-semibold text-md hover:text-black focus:text-black  md:text-basecursor-default flex items-center text-gray-700  outline-none" 
                    name="custom-input-number" 
                    v-model="task.likely"/>
                <button
                    @click="incrementNumber(featureIndex, taskIndex, 'likely')"
                    class="bg-green-300 text-green-600 hover:text-green-700 hover:bg-green-400 h-full w-10 rounded-r cursor-pointer">
                    <span class="m-auto text-2xl font-thin">+</span>
                </button>
            </div>
            <div class="flex flex-row h-10">
                <span class="text-sm border border-blue-600 rounded-l px-4 py-2 bg-blue-500 text-white whitespace-no-wrap">Pessimistic:</span>
                <button
                    @click="decrementNumber(featureIndex, taskIndex, 'pessimistic')" 
                    class="bg-red-300 text-red-600 hover:text-red-700 hover:bg-red-400 h-full w-10 cursor-pointer outline-none">
                <span class="m-auto text-2xl font-thin">−</span>
                </button>
                <input type="number" class="outline-none focus:outline-none text-center w-24 bg-gray-300 font-semibold text-md hover:text-black focus:text-black  md:text-basecursor-default flex items-center text-gray-700  outline-none" 
                    name="custom-input-number" 
                    v-model="task.pessimistic"/>
                <button
                    @click="incrementNumber(featureIndex, taskIndex, 'pessimistic')"
                    class="bg-green-300 text-green-600 hover:text-green-700 hover:bg-green-400 h-full w-10 rounded-r cursor-pointer">
                    <span class="m-auto text-2xl font-thin">+</span>
                </button>
            </div>
        </div>
    </span>
</template>
<script>
import Label from './Label.vue';
export default {
  components: { Label },
    
    props: ['features', 'featureIndex', 'taskIndex', 'task'],
    methods: {
        
        incrementNumber(index, key, value){
            return this.features[index].tasks[key][value]+= 0.5;
        },
        decrementNumber(index, key, value){
            return this.features[index].tasks[key][value]-= 0.5;
        },
    }
}
</script>
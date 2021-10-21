<template>
    <Head title="Estimation" />

    <BreezeAuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Estimation
            </h2>
        </template>


        <div class="absolute right-32 mt-12">
            <button
                @click.prevent="toggleEstimate"
                class="absolute  right-0 bg-blue-600 rounded-full p-6 text-white hover:bg-blue-700 hover:text-blue-200 z-50 border-t border-blue-800">
                <svg class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor"
                    v-if="!estimation" 
                >
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7" />
                </svg>
                <svg class="h-10 w-10" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true"
                    v-if="estimation" 
                >
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                </svg>
            </button>
            <div
                v-if="estimation" 
                class="bg-gray-700 mt-12 bg-blue-600 rounded-lg z-50  border border-blue-800">
                    <div class="p-3 text-blue-100 w-full text-center">
                        Estimation overview
                    </div>
                    <div class="grid grid-cols-2">
                        <div class = "p-2 bg-blue-500 text-white rounded">
                            <ul class="border-b border-blue-600">
                                <li class="p-2 text-white">
                                    <ul>
                                        <li class="p-1 text-lg font-bold">Admin</li>
                                        <li class="px-3 text-lg font-bold">Project management
                                            <span class="float-right text-green-300">+10%</span>
                                        </li>
                                        <li class="px-3 text-lg font-bold">Communication
                                            <span class="float-right text-green-300">+10%</span>
                                        </li>
                                        <li class="px-3 text-lg font-bold">Total extra hours
                                            <span class="float-right text-green-300">{{  (estimatedHours() * 0.2).toFixed(1) }}</span>
                                        </li>
                                    </ul>
                                </li>
                            </ul>
                            <ul class="border-b border-blue-600">
                                <li class="p-2 text-white">
                                    <ul>
                                        <li class="p-1 text-lg font-bold">Estimated hours</li>
                                        <li class="px-3 text-lg font-bold">{{ estimatedHours() }}
                                        </li>
                                    </ul>
                                </li>
                            </ul>
                            <ul class="border-b border-blue-600">
                                <li class="p-2 text-white">
                                    <ul>
                                        <li class="p-1 text-lg font-bold">Hours + admin costs</li>
                                        <li class="px-3 text-lg font-bold">{{ (estimatedHours() * 1.2).toFixed(1) }}
                                            <span class="float-right text-green-300">+20%</span>
                                        </li>
                                    </ul>
                                </li>
                            </ul>
                            <ul class="border-b border-blue-600">
                                <li class="p-2 text-white">
                                    <ul>
                                        <li class="p-1 text-lg font-bold">Blended rate</li>
                                        <li class="px-3 text-lg font-bold">£80
                                            <span class="float-right text-red-300">ex. VAT</span>
                                        </li>
                                    </ul>
                                </li>
                            </ul>
                            <ul class="border-b border-blue-600">
                                <li class="p-2 text-white">
                                    <ul>
                                        <li class="p-1 text-lg font-bold">Total Estimate</li>
                                        <li class="px-3 text-lg font-bold">£{{ ((estimatedHours() * 1.2) * 80.00).toFixed(2) }}
                                            <span class="float-right text-red-300">ex. VAT</span>
                                        </li>
                                    </ul>
                                </li>
                            </ul>
                        </div>
                        <ul>
                            <li 
                                v-for="(item, index) in features" :key="item"
                                class="p-2 text-white"
                            >
                                <ul class="border-b border-blue-500">
                                    <li class="p-1 text-lg font-bold">{{ index }}</li>
                                    <li 
                                        class="px-3 text-lg font-bold"
                                        v-for="(task, i) in features[index].tasks" :key="i"
                                    >
                                            <strike v-if="!task.inUse" class="text-red-300">&bull; {{i}}</strike>
                                            <div v-else>&bull; {{ i }}</div>
                                    </li>
                                </ul>
                            </li>
                        </ul>
                    </div>                   

            </div>
        </div>
        <div class="py-6">
            <div class="container mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="bg-blue-600 border-b border-blue-200">
                        <div>
                            <ul>
                                <li class="p-3 text-white">Phase check</li>
                            </ul>
                            <div class="bg-blue-600">
                                <nav class="flex flex-col sm:flex-row border-b">         
                                    <EstimationFeature
                                        v-for="(item, index) in features" :key="item"
                                        :index = "index"
                                        :item = "item"
                                        :toggleFeature = "toggleFeature"
                                        :features = "features"
                                    />
                                </nav>
                            </div>
                            <div>
                                <EstimationTask 
                                    v-for="(item, index) in features" :key="item"
                                    :index = "index"
                                    :item = "item"
                                    :features = "features"
                                    :hoursTotal = "hoursTotal"
                                    :standardDeviation = "standardDeviation"
                                />
                            </div>
                        </div>

                    </div>
                </div>
            </div>
        </div>

        <!---<div class="py-6">
            <div class="container mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="bg-white border-b border-gray-200">
                        <div class="block w-full overflow-x-auto">
                            <table 
                            class="items-center bg-transparent w-full border-collapse ">
                                <thead>
                                    <tr>
                                        <th class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left">
                                            Phase check
                                        </th>
                                    </tr>
                                </thead>

                                <tbody v-for="(item, index) in features" :key="item">
                                    <tr @click="toggleFeature(index)"
                                        v-bind:class="{ active: features[index].isActive, 'text-green-600': 'text-red-600' }">
                                        <th class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4 text-left text-blueGray-700 ">
                                            {{ index }}
                                        </th>
                                    </tr>
                                    <tr v-if="features[index].show">
                                        <td>
                                            <table class="items-center bg-transparent w-full border-collapse">
                                                <tr v-for="(task, i) in features[index].tasks" :key="i" >
                                                    <table v-bind:class = "!this.features[index].tasks[i].inUse?'bg-red-800':'bg-green-800'"
                                                        class="items-center bg-transparent w-full border-collapse text-white"
                                                    >
                                                        <tr>

                                                    <td class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left">
                                                        {{i}}
                                                    </td>
                                                    <td class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left">
                                                        <div class="flex flex-row h-10 rounded-lg bg-transparent mt-1 z-0">
                                                            <button
                                                                @click="decrementNumber(index, i, 'pessimistic')"
                                                                class=" bg-gray-300 text-gray-600 hover:text-gray-700 hover:bg-gray-400 h-full w-10 rounded-l cursor-pointer outline-none">
                                                            <span class="m-auto text-2xl font-thin">−</span>
                                                            </button>
                                                            <input type="number" class="w-24 outline-none focus:outline-none text-center bg-gray-300 font-semibold text-md hover:text-black focus:text-black  md:text-basecursor-default flex items-center text-gray-700  outline-none" 
                                                                name="custom-input-number" 
                                                                v-model="task.pessimistic"/>
                                                            <button
                                                                @click="incrementNumber(index, i, 'pessimistic')"
                                                                class="bg-gray-300 text-gray-600 hover:text-gray-700 hover:bg-gray-400 h-full w-10 rounded-r cursor-pointer">
                                                                <span class="m-auto text-2xl font-thin">+</span>
                                                            </button>
                                                        </div>
                                                    </td>
                                                    <td class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left">
                                                        <div class="flex flex-row h-10 w-full rounded-lg bg-transparent mt-1 z-10">
                                                            <button
                                                                @click="decrementNumber(index, i, 'likely')" 
                                                                class=" bg-gray-300 text-gray-600 hover:text-gray-700 hover:bg-gray-400 h-full w-10 rounded-l cursor-pointer outline-none">
                                                            <span class="m-auto text-2xl font-thin">−</span>
                                                            </button>
                                                            <input type="number" class="outline-none focus:outline-none text-center w-24 bg-gray-300 font-semibold text-md hover:text-black focus:text-black  md:text-basecursor-default flex items-center text-gray-700  outline-none" 
                                                                name="custom-input-number" 
                                                                v-model="task.likely"/>
                                                            <button
                                                                @click="incrementNumber(index, i, 'likely')"
                                                                class="bg-gray-300 text-gray-600 hover:text-gray-700 hover:bg-gray-400 h-full w-10 rounded-r cursor-pointer">
                                                                <span class="m-auto text-2xl font-thin">+</span>
                                                            </button>
                                                        </div>
                                                    </td>
                                                    <td class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left">
                                                        <div class="flex flex-row h-10 w-full rounded-lg bg-transparent mt-1 z-0">
                                                            <button
                                                                @click="decrementNumber(index, i, 'optimistic')"
                                                                class=" bg-gray-300 text-gray-600 hover:text-gray-700 hover:bg-gray-400 h-full w-10 rounded-l cursor-pointer outline-none">
                                                            <span class="m-auto text-2xl font-thin">−</span>
                                                            </button>
                                                            <input type="number" class="outline-none focus:outline-none text-center w-24 bg-gray-300 font-semibold text-md hover:text-black focus:text-black  md:text-basecursor-default flex items-center text-gray-700  outline-none" 
                                                                name="custom-input-number" 
                                                                v-model="task.optimistic"/>
                                                            <button
                                                                @click="incrementNumber(index, i, 'optimistic')"
                                                                class="bg-gray-300 text-gray-600 hover:text-gray-700 hover:bg-gray-400 h-full w-10 rounded-r cursor-pointer">
                                                                <span class="m-auto text-2xl font-thin">+</span>
                                                            </button>
                                                        </div>
                                                    </td>
                                                    <td class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left">
                                                        {{ hoursTotal(task.optimistic, task.pessimistic, task.likely) }}

                                                    </td>
                                                    <td class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left">
                                                        {{ standardDeviation([task.optimistic, task.pessimistic, task.likely]) }}%

                                                    </td>
                                                    <td class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left">
                                                        <button
                                                            @click.prevent="toggleUse(index, i)"
                                                            class="bg-red-600 rounded p-2 text-white hover:bg-red-700 hover:text-red-200 z-50">
                                                            
                                                            <svg class="h-10 w-10" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                                                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                                                            </svg>
                                                        </button>

                                                    </td>
                                                        </tr>
                                                    </table>
                                                </tr>
                                            </table>
                                            <div class="w-full p-3">
                                                <button class="w-full rounded bg-blue-600 text-white px-3 py-2" @click="nextPhase(index)">Complete phase</button>
                                            </div>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>---->
    </BreezeAuthenticatedLayout> 
    
</template>

<script>

import EstimationFeature from '@/Components/EstimationFeature.vue'
import EstimationTask from '@/Components/EstimationTask.vue'
import BreezeAuthenticatedLayout from '@/Layouts/Authenticated.vue'
import { Head } from '@inertiajs/inertia-vue3';

export default {
    components: {
        BreezeAuthenticatedLayout,
        Head,
        EstimationFeature,
        EstimationTask,
    },
    methods : {
        toggleFeature(index){
            this.features[index].show = !this.features[index].show
        },
        toggleEstimate(){
            this.estimation = !this.estimation
        },
        hoursTotal(o, p, l){
            return ((o + p + l*4)/6).toFixed(1);
        },
        standardDeviation(arr, usePopulation = false){
            const mean = arr.reduce((acc, val) => acc + val, 0) / arr.length;
            return (Math.sqrt(
                arr
                .reduce((acc, val) => acc.concat((val - mean) ** 2), [])
                .reduce((acc, val) => acc + val, 0) /
                (arr.length - (usePopulation ? 0 : 1))
            )*100).toFixed(0);
        },
        estimatedHours () {
            var sum = 0;
            for (const [key, value] of Object.entries(this.features)) {
                for (const [key, value] of Object.entries(this.features[key].tasks)) {
                    if(value.inUse && value.optimistic > 0.1 && value.pessimistic > 0.1 && value.likely > 0.1) {
                        sum += ((value.optimistic + value.pessimistic + value.likely*4)/6);
                        //console.log(value.optimistic, value.pessimistic, value.likely)
                    }

                }
            }
            return (sum).toFixed(1);
        },
        nextPhase(current) {
            if (!Array.prototype.last){
                Array.prototype.last = function(){
                    return this[this.length - 1];
                };
            };

            if(current === Object.keys(this.features).last()){
                //Add a move on to next section button... Blocked because its the last element to go through.
                console.log('Moving onto final estimation')
                this.estimation = true;
            }
            else {
                let keys = Object.keys(this.features);
                let nextIndex = keys.indexOf(current) +1;
                let nextItem = keys[nextIndex];
                console.log(Object.keys(this.features).last(), current)

                this.features[nextItem].show = true;
                this.features[current].show = false;
                this.features[current].isActive = true;
            }
        }
    },
    data: function () {
        return {
            estimation : false,
            features : {
                'Requirements Discovery' : {
                    'show' : false,
                    'isActive': false,
                    'hasError':false,
                    'tasks' :  {
                        'Meeting with key users of system' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'Review any existing system' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": true
                        },
                    }
                },               
                'Technical Research' : {
                    'show' : false,
                    'isActive': false,
                    'hasError':false,
                    'tasks' :  {
                        'Test proposed modules/plugins' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'Review existing codebase to assess risks' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": true
                        },
                        'Review proposed systems with client' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'Test integrations & prototyping ideas' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": true
                        },
                    },
                }, 
                'Information Architecture' : {
                    'show' : false,
                    'isActive': false,
                    'hasError':false,
                    'tasks' :  {
                        'User Personas' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'User Journeys' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'Site Map' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'UI Sketches' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'Wireframes' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'Process Flows' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'User Stories' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'Content Inventories' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'Entity Relationship Diagram (ERD)' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'Functionality Specification Doc (FSD)' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": true
                        }
                    },
                },   
                'Design' : {
                    'show' : false,
                    'isActive': false,
                    'hasError':false,
                    'tasks' :  {
                        'High-fidelity mockups' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": true
                        },
                        'Interactive Prototypes' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        }
                    }, 
                },
                'Planning' : {
                    'show' : false,
                    'isActive': false,
                    'hasError':false,
                    'tasks' :  {
                        'Creating tasks with sprints and milestones' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": true
                        },
                        'Risk assessment' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": false
                        },
                        'Estimation' : {
                            "show" : false,
                            "pessimistic": 0.0,
                            "likely" : 0.0,
                            "optimistic" : 0.0,
                            "inUse": true
                        }
                    }
                }
            }
        }
    },
}
</script>

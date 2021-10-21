<template>
    <Head title="Dashboard" />

    <BreezeAuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Dashboard
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 bg-white border-b border-gray-200">
                        <table>
                            <thead>
                                <tr>
                                    <td>feature</td>
                                </tr>
                            </thead>
                            <tbody>
                                <tr>
                                    <td></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </BreezeAuthenticatedLayout>
</template>

<script>
import BreezeAuthenticatedLayout from '@/Layouts/Authenticated.vue'
import { Head } from '@inertiajs/inertia-vue3';

export default {
    components: {
        BreezeAuthenticatedLayout,
        Head,
    },
    data: function () {
        return {
            features : {
                'Requirements Discovery' : 
                    [
                        'Meeting with key users of system', 
                        'Review any existing system'
                    ],
                'Technical Research' : 
                    [
                        'Test proposed modules/plugins',
                        'Review existing codebase to assess risks',
                        'Review proposed systems with client',
                        'Test integrations & prototyping ideas'
                    ],
                'Information Architecture' : 
                    [
                        'User Personas',
                        'User Journeys',
                        'Site Map',
                        'UI Sketches',
                        'Wireframes',
                        'Process Flows',
                        'User Stories',
                        'Content Inventories',
                        'Entity Relationship Diagram (ERD)',
                        'Functionality Specification Doc (FSD)'
                    ],
                'Design' :
                    [
                        'High-fidelity mockups',
                        'Interactive Prototypes'
                    ],
                'Planning' :
                    [
                        'Creating tasks with sprints and milestones',
                        'Risk assessment',
                        'Estimation'
                    ]
            }
        }
    },
}
</script>

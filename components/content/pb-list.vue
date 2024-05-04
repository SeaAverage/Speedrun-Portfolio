<template>
    <div class="not-prose">
        <section v-if="pending">Loading...</section>
        <section v-else-if="error">Someting went wrong... Try again!</section>
        <section v-else>
            <div class="grid justify-items-end">
                <div class="flex justify-items-end space-x-2 mb-8">
                    <div>Sort By:</div>
                    <USelectMenu class="w-20" v-model="selected" :options="sorters" :popper="{ arrow: true }" />
                </div>
            </div>
            <ul class="grid grid-cols-1 gap-4">
                <li v-for="personalBest in pbs" :key="personalBest.run.id"
                    class="border border-gray-200 rounded-sm p-4 hover:bg-gray-100 dark:hover:bg-gray-600 font-mono">
                    <a :href="personalBest.run.weblink" target="_blank">
                        <div class="flex items-center justify-between text-sm">
                            <div class="font-semibold">{{ personalBest.game.data.names.international }}</div>
                            <div>{{ personalBest.place }} 🏆</div>
                        </div>
                        <div class="flex items-center justify-between text-sm">
                            <div class="font-semibold">{{ personalBest.category.data.name }}</div>
                            <div class="font-semibold">{{ convertTime(personalBest.run.times.primary_t) }}</div>
                        </div>
                    </a>
                </li>
            </ul>
        </section>
    </div>
</template>

<script setup lang="ts">

    const sorters = [
        'A-Z',
        'Rank',
        'Time'
    ]
    const selected = ref(sorters[0])



    const { error, pending, data } = await useFetch('https://www.speedrun.com/api/v1/users/7j4p63wj/personal-bests?embed=game,category');

    const pbs = computed(
        () => {
            const clonedData = [...data.value.data]
            return clonedData.sort((a: any, b: any) => {
                if (selected.value === 'Rank') {
                    a = a.place
                    b = b.place
                }
                if (selected.value === 'Time') {
                    a = a.run.times.primary_t
                    b = b.run.times.primary_t
                }
                if (selected.value === 'A-Z') {
                    a = a.game.data.names.international
                    b = b.game.data.names.international
                }
                if (a < b) return -1
                if (a > b) return 1
                return 0
            })
        }
    )

</script>

<script lang="ts">
    import prettyMilliseconds from 'pretty-ms';

    function convertTime(pbTime: number) {
        let millisecondTime = pbTime * 1000
        let convertedTime = prettyMilliseconds(millisecondTime, { colonNotation: true })
        return convertedTime
    }

</script>

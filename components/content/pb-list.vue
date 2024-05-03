<template>
    <div class="not-prose">
        <section v-if="pending">Loading...</section>
        <section v-else-if="error">Someting went wrong... Try again!</section>
        <section v-else>
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


    const { error, pending, data } = await useFetch('https://www.speedrun.com/api/v1/users/7j4p63wj/personal-bests?embed=game,category');

    const pbs = computed(
        () => data.value?.data.sort((a: any, b: any) => a.place - b.place)
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

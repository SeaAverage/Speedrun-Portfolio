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
                            <div class="font-semibold">{{ convertTime(personalBest.run.times.primary) }}</div>
                        </div>
                    </a>
                </li>
            </ul>
        </section>
    </div>
</template>

<script setup lang="ts">
    import prettyMilliseconds from 'pretty-ms';



    const { error, pending, data } = await useFetch('https://www.speedrun.com/api/v1/users/7j4p63wj/personal-bests?embed=game,category');

    const pbs = computed(
        () => data.value?.data.sort((a, b) => a.place - b.place)
    )

</script>

<script lang="ts">
    function convertTime(pbTime) {
        const timeInterpreter = require("@onereach/time-interpreter");
        const converter = new timeInterpreter();
        let convertedTime1 = converter.formatDuration(pbTime)
        let convertedTime2 = prettyMilliseconds(convertedTime1, { colonNotation: true })
        return convertedTime2
    }

</script>

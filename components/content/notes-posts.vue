<template>
    <section class="not-prose font-mono">
        <div class="column text-gray-400 text-sm">
            <div>Game</div>
            <div>Category</div>
        </div>
        <ul>
            <li v-for="post in posts" :key="post._path">
                <NuxtLink :to="post._path" class="column hover:bg-gray-100 dark:hover:bg-gray-800">
                    <div :class="{ 'text-transparent': !post.displayYear }">
                        {{ post.game }}</div>
                    <div>{{ post.category }}</div>
                </NuxtLink>
            </li>
        </ul>
    </section>
</template>

<script setup>
    const { data } = await useAsyncData(
        'note-list',
        () => queryContent('/notes')
            .where({ _path: { $ne: '/notes' } })
            .only(['_path', 'game', 'category'])
            .sort({ game: 1 })
            .find()
    )

    const posts = computed(() => {
        if (!data.value) {
            return []
        }
        const result = []
        let lastYear = null

        for (const post of data.value) {
            const year = new Date(post.publishedAt).getFullYear()
            const displayYear = year !== lastYear
            post.displayYear = displayYear
            post.year = year
            result.push(post)
            lastYear = year
        }
        return result
    })
</script>

<style scoped>
    .column {
        @apply flex justify-between ml-auto mr-auto max-w-2xl py-2 border-b border-gray-200 dark:border-gray-700
    }
</style>
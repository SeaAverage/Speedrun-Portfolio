<template>
    <nav class="font-mono">
        <ul class="flex space-x-4">
            <li>
                <NuxtLink to="/" class="link">Main</NuxtLink>
            </li>
            <!-- <li>
                <NuxtLink to="/about" class="link">About</NuxtLink>
            </li> -->
            <li>
                <NuxtLink to="/notes" class="link">Notes</NuxtLink>
            </li>
            <li>
                <NuxtLink to="/projects" class="link">Personal Bests</NuxtLink>
            </li>
        </ul>
    </nav>
</template>

<style scoped>
    .link {
        @apply p-1 hover:bg-gray-200 dark:hover:bg-gray-600
    }
</style>
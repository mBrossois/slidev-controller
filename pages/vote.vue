<template>
    <UContainer class="flex flex-col gap-4 justify-center items-center w-full h-screen lg:flex-row">
        <ClientOnly>
            <Connect :room="roomName" />
        </ClientOnly>
    </UContainer>
</template>

<script setup lang="ts">
const route = useRoute()

const roomName = route.query.room
</script>
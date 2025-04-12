<template>
    <UButton class="text-8xl bg-gray-600 cursor-pointer hover:bg-gray-800" @click="sendThumbsUp">
        👍
    </UButton>
    <UButton class="text-8xl bg-gray-600 cursor-pointer hover:bg-gray-800" @click="sendRaisedHand">
        ✋
    </UButton>
</template>

<script setup lang="ts">
import PartySocket from 'partysocket';

const props = defineProps<{
    room: string
}>()

const runtimeConfig = useRuntimeConfig()

const partySocket = new PartySocket({
  host: runtimeConfig.public.partykitUrl,
  room: props.room
});

function sendThumbsUp() {
    partySocket.send('thumbs-up')
}


function sendRaisedHand() {
    partySocket.send('raised-hand')
}

</script>
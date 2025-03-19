<template>
    <UButton color="primary" @click="vote('upvote')">
        <UIcon name="ic:baseline-thumb-up" size="100"/>
    </UButton>
    <UButton color="error" @click="vote('downvote')">
        <UIcon name="ic:baseline-thumb-down" size="100"/>
    </UButton>
    <div class="bottom-bar">
        <UIcon v-for="upvote in upvotes" :key="upvote" class="fly-up" name="ic:baseline-thumb-up"  size="24"/>
        <UIcon v-for="downvote in downvotes" :key="downvote" class="fly-up" name="ic:baseline-thumb-down"  size="24"/>
    </div>
</template>

<script setup lang="ts">
import PartySocket from 'partysocket';

const partySocket = new PartySocket({
  host: 'host-adres',
  room: 'roomies'
});

const upvotes: Ref<Array<number>> = ref([])
let currUpvote = 0
function addUpvote() {
    const id = currUpvote
    currUpvote += 1
    upvotes.value.push(id)
    setTimeout(() => {
        upvotes.value = upvotes.value.filter(value => value !== id)
    }, 5000)
}

const downvotes: Ref<Array<number>> = ref([])
let currDownvote = 0
function addDownvote() {
    const id = currDownvote
    currDownvote += 1
    downvotes.value.push(id)
    setTimeout(() => {
        downvotes.value = downvotes.value.filter(value => value !== id)
    }, 5000)
}

partySocket.addEventListener("message", (e) => {
  const message = JSON.parse(e.data)
  if(message.messageType === 'vote') {
    if(message.value === 'upvote') {
        addUpvote()
    }
    if(message.value === 'downvote') {
        addDownvote()
    }
  }
});

function vote(value: string) {
    if(value === 'upvote') {
        addUpvote()
    }
    if(value === 'downvote') {
        addDownvote()
    }
    partySocket.send(JSON.stringify({messageType: 'vote', value}))
}
</script>

<style scoped>
.fly-up {
    position: absolute;
    bottom: -16px;

    animation: up 2500ms ease-in;
}

@keyframes up {
    from {
        bottom: -40px;
    }
    to {
        bottom: 100vh;
    }
}
</style>
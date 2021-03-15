<template>
    <div v-if="events.length">
        <div v-for="event in events" :key="event.id" class="events-container">
            <SingleEvent :event = "event" @deleteEvent="handleDelete" />
        </div>
    </div>

    <div v-else>
        <p>Loading Events...</p>
    </div>

</template>

<script>
const axios = require("axios")
import SingleEvent from "./SingleEvent"

export default {
    components: { SingleEvent },
    data() {
        return {
            events: [],
            // showDetails: false
        }
    },
    mounted() {
        let self = this
        async function getEventsData() {
            try {
                const result = await axios.get("https://challenge.pluralo.com/event/anonym/getall")
                
                const eventsData = result.data.events
                console.log(eventsData)
                self.events = eventsData
                
            } catch (error) {
                console.log(error)
            }
        }

        getEventsData()        
    }, 
    methods: {
        btnChangeState() {
            this.showDetails = !this.showDetails
            this.$refs.btnToggle.innerHTML = "Close"
        },
        handleDelete(eventId) {
            this.events = this.events.filter((event) => {
                return event.id != eventId
            })
        }
            
        
    }

}
</script>

<style>
.low {
    background: red;
}

.medium {
    background: yellow;
}

.high {
    background: green;
}
</style>
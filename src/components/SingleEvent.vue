<template>
    <div class="event">
        <div class="event-short">
            <div class="event-name-container">
                <div class="event-title"> {{ event.name }}</div>
                <div class="event-dates"> {{ event.eventStart }} from {{ event.eventEnd }} </div>
            </div>
            <div class="event-availability-container">
                <h1>Availability</h1>
                <p class="event-availability">{{event.booked}}/{{event.capacity}}</p>
            </div>

            <button class="show-btn" @click="btnChangeState" ref="btnToggle">Open</button>
        </div>
        <div v-if="showDetails" class="details-container">
            <div class="event-details-text">Lorem ipsum dolor sit amet consectetur, adipisicing elit. Illum, suscipit.</div>
            <button class="btn-edit"><i class="far fa-edit"></i>Edit</button>
            <button class="btn-delete" @click="deleteEvent"><i class="far fa-trash-alt"></i>Delete</button>
        </div>

        <div v-if="showEditDetails" class="details-container">
            <div class="event-details-text">Lorem ipsum dolor sit amet consectetur, adipisicing elit. Illum, suscipit.</div>
            <button class="btn-save"><i class="far fa-save"></i>Save</button>
            <button class="btn-cancel"><i class="far fa-meh"></i>Cancel</button>
            <button class="btn-delete" @click="deleteEvent"><i class="far fa-trash-alt"></i>Delete</button>
        </div>
    </div>

</template>

<script>
const axios = require("axios")

export default {
    props: ["event"],
    data() {
        return {
            showDetails: true,
            showEditDetails: true,
            uri: "https://challenge.pluralo.com/event/anonym/"
        }
    },
    methods: {
        btnChangeState() {
            this.showDetails = !this.showDetails
            this.$refs.btnToggle.innerText = this.showDetails ? "Hide" : "Open"
        },
        deleteEvent() {
            let self = this
            const eventId = self.event.id
            async function deleteEventData() {
                try {
                    
                    const result = await axios.post("https://challenge.pluralo.com/event/anonym/delete", { "id": eventId })
                    console.log(result)

                    self.$emit("deleteEvent", eventId)
                    
                } catch (error) {
                    console.log(error)
                }
            }

            deleteEventData()
        }
    }
}
</script>

<style>
.event {
    margin: 20px auto;
    background: rgb(240, 244, 252);
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.5);
    border-left: 4px solid #e90074;
}

button {
    padding: 10px 20px;
    color: white;
    border-radius: 4px;
    font-size: 14px;
    cursor: pointer;
}

.show-btn {
    background: #F1F1F8;
    border: 1px solid #D3D7DB;
    color: #4B536B;
}
.btn-edit {
    background: #00A9F0;
    border: 1px solid rgba(74,82,106,0.35);
}
.btn-save {
    background: #6AC951;
    border: 1px solid rgba(75,83,107,0.4);
}
.btn-cancel {
    color: #4B536B;
    border: 1px solid #D3D7DB;
}
.btn-cancel i {
    color: #4B536B;
}

.btn-delete {
    background: #F16C64;
    border: 1px solid rgba(74,82,106,0.35);
}


i {
    color: white;
    padding-right: 5px;
}
</style>
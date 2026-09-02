<template>
    <v-container>
        <v-row>
            <v-col>
                <v-sheet class="pa-8" elevation="6">
                    <h1>Office Hours Calendar</h1>
                    <div style="margin-top: 2em;">
                        <vue-cal style="height: 1000px;" :events="events" :disable-views="['years', 'year']"
                            :time-cell-height="100" active-view="month" events-on-month-view=true />
                    </div>
                </v-sheet>
            </v-col>
        </v-row>
    </v-container>
</template>

<script lang="ts">
import VueCal from 'vue-cal'
import 'vue-cal/dist/vuecal.css'

import { defineComponent } from "vue";

function time_number_to_string(time: number) {
    if (time - Math.floor(time) < 0.5) {
        return String(Math.floor(time)) + ":00"
    } else {
        return String(Math.floor(time)) + ":30"
    }
}

var recesses = [
    {
        start: new Date(2026, 9, 21),
        end: new Date(2026, 9, 27)
    },
    {
        start: new Date(2026, 10, 21),
        end: new Date(2026, 10, 30)
    }
]

function parseLocalDate(date: string) {
    const [year, month, day] = date.split("-").map(Number)
    return new Date(year, month - 1, day)
}

function getRecurringEventsStartEnd(start: string, end: string, day: number, name: string, loc: string, zoom_link: string, start_time: number, end_time: number) {
    var dates: { [key: string]: string }[] = []
    var start_date = parseLocalDate(start)
    var end_date = parseLocalDate(end)
    start_date.setDate(start_date.getDate() + ((day + 7 - start_date.getDay()) % 7))

    while (start_date <= end_date) {
        var date = start_date.getFullYear() + '-' + String(start_date.getMonth() + 1).padStart(2, '0') + '-' + String(start_date.getDate()).padStart(2, '0')
        let in_recess = false
        for (let recess of recesses) {
            if (start_date.getTime() >= recess.start.getTime() && start_date.getTime() < recess.end.getTime()) {
                in_recess = true
                break
            }
        }

        var contents: string[] = []
        if (loc) {
            contents.push(loc)
        }
        if (zoom_link) {
            contents.push(`<a href=${zoom_link}>zoom</a>`)
        }

        if (!in_recess) {
            dates.push({
                title: name,
                content: contents.join("<br>"),
                start: date + ' ' + time_number_to_string(start_time),
                end: date + ' ' + time_number_to_string(end_time),
            })
        }

        start_date.setDate(start_date.getDate() + 7)
    }
    return dates
}

function getRecurringEvents(day: number, name: string, loc: string, zoom_link: string, start_time: number, end_time: number) {
    return getRecurringEventsStartEnd("2026-09-03", "2026-12-11", day, name, loc, zoom_link, start_time, end_time)
}

function getRexEvents() {
    return getRecurringEvents(2, "Rex Ying", "17 Hillhouse, Room 332", "", 15, 16)
}

function getNgocEvents() {
    return getRecurringEvents(5, "Ngoc Bui", "Dunham 432", "", 14.5, 15.5)
}

function getEvents() {
    return getRexEvents()
        .concat(getNgocEvents())
}


export default defineComponent({
    components: { VueCal },
    data: () => ({
        events: getEvents()
    })
})
</script>

<style>

.vuecal__event-title {
  font-size: 1.2em;
  font-weight: bold;
  /* margin: 4px 0 8px; */
}

.vuecal__event-time {
  display: inline-block;
  /* margin-bottom: 12px;
  padding-bottom: 12px; */
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
}

.vuecal__event-content {
  font-style: italic;
  font-size: 0.8em;
}
</style>

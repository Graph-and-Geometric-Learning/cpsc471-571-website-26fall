<template>
  <v-container>
    <v-row>
      <v-col>
        <v-sheet class="pa-8" elevation="6">
          <h1>Fall 2026 Course Schedule</h1>
          <p class="mb-6">
            Classes meet on Tuesdays and Thursdays. Lecture topics, readings,
            assignments, and deadlines will be added as the semester progresses.
            Yale College classes end on Friday, December 11; our final regular
            class meeting is Thursday, December 10.
          </p>

          <v-table>
            <thead>
              <tr>
                <th>Week</th>
                <th>Date</th>
                <th>Topic</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in items" :key="item.date">
                <td><b>{{ item.week }}</b></td>
                <td>{{ item.date }}</td>
                <td :class="{ 'text-grey': item.noClass }">
                  {{ item.topic }}
                </td>
              </tr>
            </tbody>
          </v-table>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface ScheduleItem {
  week: number;
  date: string;
  topic: string;
  noClass?: boolean;
}

const classMeeting = (week: number, date: string): ScheduleItem => ({
  week,
  date,
  topic: "To be announced",
});

const noClass = (week: number, date: string, topic: string): ScheduleItem => ({
  week,
  date,
  topic,
  noClass: true,
});

const items: ScheduleItem[] = [
  classMeeting(1, "Thu, Sep 3"),
  classMeeting(2, "Tue, Sep 8"),
  classMeeting(2, "Thu, Sep 10"),
  classMeeting(3, "Tue, Sep 15"),
  classMeeting(3, "Thu, Sep 17"),
  classMeeting(4, "Tue, Sep 22"),
  classMeeting(4, "Thu, Sep 24"),
  classMeeting(5, "Tue, Sep 29"),
  classMeeting(5, "Thu, Oct 1"),
  classMeeting(6, "Tue, Oct 6"),
  classMeeting(6, "Thu, Oct 8"),
  classMeeting(7, "Tue, Oct 13"),
  classMeeting(7, "Thu, Oct 15"),
  classMeeting(8, "Tue, Oct 20"),
  noClass(8, "Thu, Oct 22", "No class — October recess"),
  classMeeting(9, "Tue, Oct 27"),
  classMeeting(9, "Thu, Oct 29"),
  classMeeting(10, "Tue, Nov 3"),
  classMeeting(10, "Thu, Nov 5"),
  classMeeting(11, "Tue, Nov 10"),
  classMeeting(11, "Thu, Nov 12"),
  classMeeting(12, "Tue, Nov 17"),
  classMeeting(12, "Thu, Nov 19"),
  noClass(13, "Tue, Nov 24", "No class — November recess"),
  noClass(13, "Thu, Nov 26", "No class — Thanksgiving recess"),
  classMeeting(14, "Tue, Dec 1"),
  classMeeting(14, "Thu, Dec 3"),
  classMeeting(15, "Tue, Dec 8"),
  classMeeting(15, "Thu, Dec 10"),
];

export default defineComponent({
  name: "Syllabus",
  data: () => ({ items }),
});
</script>

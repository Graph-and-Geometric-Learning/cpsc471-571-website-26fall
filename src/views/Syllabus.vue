<template>
  <v-container>
    <v-row>
      <v-col>
        <v-sheet class="pa-8" elevation="6">
          <v-table>
            <thead>
              <tr>
                <th>Week</th>
                <th>Date</th>
                <th>Lecture</th>
                <th>Slides</th>
                <th>Readings</th>
                <th>Deadlines</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in items" :key="item.date">
                <td><b>{{ item.week }}</b></td>
                <td>{{ item.date }}</td>
                <td :class="{ 'text-grey': item.noClass }">
                  {{ item.lecture }}
                </td>
                <td>
                  <a
                    v-if="item.slides"
                    :href="materialUrl(item.slides)"
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    Slides
                  </a>
                </td>
                <td>
                  <div v-if="item.readings?.length" class="d-flex flex-column align-start ga-1">
                    <a
                      v-for="reading in item.readings"
                      :key="reading.href"
                      :href="materialUrl(reading.href)"
                      target="_blank"
                      rel="noopener noreferrer"
                    >
                      {{ reading.label }}
                    </a>
                  </div>
                </td>
                <td></td>
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
  lecture: string;
  slides?: string;
  readings?: Reading[];
  noClass?: boolean;
}

interface Reading {
  label: string;
  href: string;
}

interface LectureMaterials {
  slides?: string;
  readings?: Reading[];
}

const classMeeting = (
  week: number,
  date: string,
  lecture: string,
  materials: LectureMaterials = {},
): ScheduleItem => ({
  week,
  date,
  lecture,
  ...materials,
});

const noClass = (week: number, date: string, lecture: string): ScheduleItem => ({
  week,
  date,
  lecture,
  noClass: true,
});

const items: ScheduleItem[] = [
  classMeeting(
    1,
    "Thu, Sep 3",
    "Introduction to Trustworthy AI",
    {
      slides: "lectures/2026-fall/01-introduction-to-trustworthy-ai.pdf",
      readings: [
        {
          label: "AI Sustainability",
          href: "https://arxiv.org/pdf/2205.03824",
        },
      ],
    },
  ),
  classMeeting(2, "Tue, Sep 8", "Deep Learning Basics, CNNs, and RNNs"),
  classMeeting(2, "Thu, Sep 10", "Transformers and Large Language Models (LLMs)"),
  classMeeting(3, "Tue, Sep 15", "Explainability of Neural Networks (XAI)"),
  classMeeting(3, "Thu, Sep 17", "Local Explainability"),
  classMeeting(4, "Tue, Sep 22", "Explainability Evaluation"),
  classMeeting(4, "Thu, Sep 24", "Global Explainability"),
  classMeeting(5, "Tue, Sep 29", "LLM Interpretability"),
  classMeeting(5, "Thu, Oct 1", "Introduction to Adversarial Attacks"),
  classMeeting(6, "Tue, Oct 6", "Evasion Attacks and Defenses"),
  classMeeting(6, "Thu, Oct 8", "In-class work session"),
  classMeeting(7, "Tue, Oct 13", "Poisoning Attacks and Defenses"),
  classMeeting(7, "Thu, Oct 15", "Exploratory Attacks and Defenses"),
  classMeeting(8, "Tue, Oct 20", "Verification and Robust Reinforcement Learning"),
  noClass(8, "Thu, Oct 22", "No class — October recess"),
  classMeeting(9, "Tue, Oct 27", "LLM Robustness"),
  classMeeting(9, "Thu, Oct 29", "Differential Privacy"),
  classMeeting(10, "Tue, Nov 3", "Machine Unlearning"),
  classMeeting(10, "Thu, Nov 5", "Federated Learning"),
  classMeeting(11, "Tue, Nov 10", "LLM Privacy"),
  classMeeting(11, "Thu, Nov 12", "Algorithmic Fairness in ML"),
  classMeeting(12, "Tue, Nov 17", "Fairness in LLMs"),
  classMeeting(12, "Thu, Nov 19", "Efficiency"),
  noClass(13, "Tue, Nov 24", "No class — November recess"),
  noClass(13, "Thu, Nov 26", "No class — Thanksgiving recess"),
  classMeeting(14, "Tue, Dec 1", "Guest Lecture by Prof. Arman Cohan on Security and Privacy of LLMs"),
  classMeeting(14, "Thu, Dec 3", "Revise and Prepare for Exam"),
  classMeeting(15, "Tue, Dec 8", "Guest Lecture"),
  classMeeting(15, "Thu, Dec 10", "Exam"),
];

export default defineComponent({
  name: "Syllabus",
  data: () => ({ items }),
  methods: {
    materialUrl(href: string): string {
      if (/^https?:\/\//i.test(href)) {
        return href;
      }

      return `${import.meta.env.BASE_URL}${href.replace(/^\/+/, "")}`;
    },
  },
});
</script>

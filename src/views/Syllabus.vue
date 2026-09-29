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
                <th class="readings-column">Readings</th>
                <th>Assignments</th>
                <th class="deadlines-column">Deadlines</th>
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
                <td class="readings-column">
                  <div v-if="item.readings?.length" class="reading-list">
                    <a
                      v-for="reading in item.readings"
                      :key="reading.href"
                      :href="materialUrl(reading.href)"
                      target="_blank"
                      rel="noopener noreferrer"
                      class="reading-item"
                    >
                      <span>{{ reading.label }}</span>
                    </a>
                  </div>
                </td>
                <td>
                  <div v-if="item.assignments?.length" class="d-flex flex-column align-start ga-1">
                    <a
                      v-for="assignment in item.assignments"
                      :key="assignment.href"
                      :href="materialUrl(assignment.href)"
                      :download="assignment.download"
                      :target="assignment.download ? undefined : '_blank'"
                      :rel="assignment.download ? undefined : 'noopener noreferrer'"
                    >
                      {{ assignment.label }}
                    </a>
                  </div>
                </td>
                <td class="deadlines-column">
                  <div v-if="item.deadlines?.length" class="deadline-list">
                    <div
                      v-for="deadline in item.deadlines"
                      :key="deadline"
                      class="deadline-item"
                      :class="`deadline-item--${deadlineKind(deadline)}`"
                    >
                      <span class="deadline-status">
                        {{ deadlineKind(deadline) }}
                      </span>
                      <span>{{ deadlineTitle(deadline) }}</span>
                    </div>
                  </div>
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
  lecture: string;
  slides?: string;
  readings?: MaterialLink[];
  assignments?: MaterialLink[];
  deadlines?: string[];
  noClass?: boolean;
}

interface MaterialLink {
  label: string;
  href: string;
  download?: string;
}

interface LectureMaterials {
  slides?: string;
  readings?: MaterialLink[];
  assignments?: MaterialLink[];
  deadlines?: string[];
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

const noClass = (
  week: number,
  date: string,
  lecture: string,
  materials: LectureMaterials = {},
): ScheduleItem => ({
  week,
  date,
  lecture,
  noClass: true,
  ...materials,
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
  classMeeting(
    2,
    "Tue, Sep 8",
    "Deep Learning Basics, CNNs, and RNNs",
    {
      slides: "lectures/2026-fall/02-basics.pdf",
      readings: [
        {
          label: "Trust worthy machine learning Book chapter 1.1",
          href: "http://www.trustworthymachinelearning.com",
        },
      ],
      assignments: [
        {
          label: "Written HW 1",
          href: "homework/2026-fall/written-hw-1.pdf",
        },
      ],
    },
  ),
  classMeeting(
    2,
    "Thu, Sep 10",
    "Foundational Models",
    {
      slides: "lectures/2026-fall/03-transformers.pdf",
      readings: [
        {
          label: "TrustLLMs",
          href: "https://arxiv.org/abs/2401.05561",
        },
      ],
      assignments: [
        {
          label: "Coding Homework 1",
          href: "homework/2026-fall/CPSC4710_hw1_2026.ipynb",
          download: "CPSC4710_hw1_2026.ipynb",
        },
        {
          label: "Image Files",
          href: "homework/2026-fall/CPSC4710_hw1_2026_images.zip",
          download: "CPSC4710_hw1_2026_images.zip",
        },
      ],
    },
  ),
  classMeeting(3, "Tue, Sep 15", "Explainability of Neural Networks (XAI)",
    {
      slides: "lectures/2026-fall/04-explainability.pdf",
      readings: [
        {
          label: "Integrated Gradients",
          href: "https://arxiv.org/abs/1703.01365",
        },
      ],
    }
  ),
  classMeeting(3, "Thu, Sep 17", "Local Explainability",
    {
      slides: "lectures/2026-fall/05-surrogates.pdf",
      readings: [
        {
          label: "LIME",
          href: "https://homes.cs.washington.edu/~marcotcr/blog/lime/",
        },
        {
          label: "SHAP",
          href: "https://arxiv.org/abs/1705.07874",
        },
      ],
    }
  ),
  classMeeting(4, "Tue, Sep 22", "Explainability Evaluation",
  {
    slides: "lectures/2026-fall/06-explainability_eval.pdf",
    readings: [
      {
        label: "Explanations can also be vulnerable to adversarial attacks",
        href: "https://arxiv.org/pdf/1710.10547",
      },
      {
        label: "Evaluating Explanations",
        href: "https://arxiv.org/pdf/2005.00631",
      }
    ],
  }
  ),
  classMeeting(4, "Thu, Sep 24", "Evaulating Explanations continued"),
  classMeeting(5, "Tue, Sep 29", "LLM Interpretability",
  {
    slides: "lectures/2026-fall/07_mechanistic_Interpretability_for_LLMs.pdf",
  }
  ),
  classMeeting(
    5,
    "Thu, Oct 1",
    "Introduction to Adversarial Attacks",
    {
      deadlines: [
        "Written HW 1 due",
        "Written HW 2 released (tentative)",
      ],
    },
  ),
  classMeeting(6, "Tue, Oct 6", "Evasion Attacks and Defenses"),
  classMeeting(
    6,
    "Thu, Oct 8",
    "In-class work session",
    {
      deadlines: [
        "Coding Homework 1 due",
        "Coding Homework 2 released (tentative)",
      ],
    },
  ),
  classMeeting(7, "Tue, Oct 13", "In class brainstorming session"),
  classMeeting(
    7,
    "Thu, Oct 15",
    "Hands on coding session",
    {
      deadlines: [
        "Written HW 2 due (tentative)",
        "Written HW 3 released (tentative)",
      ],
    },
  ),
  classMeeting(8, "Tue, Oct 20", "Quiz"),
  noClass(8, "Thu, Oct 22", "No class — October recess"),
  noClass(
    8,
    "Wed, Oct 21",
    "No class",
    {
      deadlines: ["Project proposal due (tentative)"],
    },
  ),
  classMeeting(
    9,
    "Tue, Oct 27",
    "Guest Lecture",
    {
      deadlines: ["Coding Homework 2 due (tentative)"],
    },
  ),
  classMeeting(
    9,
    "Thu, Oct 29",
    "Differential Privacy",
    {
      deadlines: [
        "Written HW 3 due (tentative)",
        "Written HW 4 released (tentative)",
      ],
    },
  ),
  classMeeting(10, "Tue, Nov 3", "Machine Unlearning"),
  classMeeting(
    10,
    "Thu, Nov 5",
    "Federated Learning",
    {
      deadlines: ["Coding Homework 3 released (tentative)"],
    },
  ),
  classMeeting(11, "Tue, Nov 10", "LLM Privacy"),
  classMeeting(11, "Thu, Nov 12", "Algorithmic Fairness in ML"),
  classMeeting(
    12,
    "Tue, Nov 17",
    "Fairness in LLMs",
    {
      deadlines: ["Written HW 4 due (tentative)"],
    },
  ),
  classMeeting(
    12,
    "Thu, Nov 19",
    "Agent Safety/Alignment",
    {
      deadlines: ["Coding Homework 3 due (tentative)"],
    },
  ),
  noClass(
    12,
    "Fri, Nov 20",
    "No class",
    {
      deadlines: ["Project milestone due (tentative)"],
    },
  ),
  noClass(13, "Tue, Nov 24", "No class — November recess"),
  noClass(13, "Thu, Nov 26", "No class — Thanksgiving recess"),
  classMeeting(14, "Tue, Dec 1", "Guest Lecture"),
  classMeeting(14, "Thu, Dec 3", "Guest Lecture"),
  classMeeting(15, "Tue, Dec 8", "Revise and Prepare for Exam"),
  classMeeting(15, "Thu, Dec 10", "Exam"),
  noClass(
    16,
    "Wed, Dec 16",
    "No class",
    {
      deadlines: ["Final project report due (tentative; strict deadline)"],
    },
  ),
];

export default defineComponent({
  name: "Syllabus",
  data: () => ({ items }),
  methods: {
    deadlineKind(deadline: string): "due" | "released" {
      return deadline.toLowerCase().includes("released") ? "released" : "due";
    },
    deadlineTitle(deadline: string): string {
      return deadline.replace(/\s+(due|released)(?=\s|\()/i, "");
    },
    materialUrl(href: string): string {
      if (/^https?:\/\//i.test(href)) {
        return href;
      }

      return `${import.meta.env.BASE_URL}${href.replace(/^\/+/, "")}`;
    },
  },
});
</script>

<style scoped>
.readings-column {
  min-width: 260px;
}

.reading-list {
  display: grid;
  gap: 8px;
  padding: 6px 0;
}

.reading-item {
  display: block;
  padding: 8px 10px;
  border: 1px solid rgba(var(--v-theme-primary), 0.25);
  border-radius: 6px;
  background: rgba(var(--v-theme-primary), 0.06);
  line-height: 1.35;
  text-decoration: none;
}

.reading-item:hover {
  border-color: rgb(var(--v-theme-primary));
  background: rgba(var(--v-theme-primary), 0.1);
}

.deadlines-column {
  min-width: 250px;
}

.deadline-list {
  display: grid;
  gap: 8px;
  padding: 6px 0;
}

.deadline-item {
  display: grid;
  grid-template-columns: 68px 1fr;
  gap: 8px;
  align-items: start;
  padding: 8px 10px;
  border-left: 4px solid;
  border-radius: 6px;
  font-size: 0.875rem;
  line-height: 1.35;
}

.deadline-item--due {
  border-color: rgb(var(--v-theme-error));
  background: rgba(var(--v-theme-error), 0.1);
}

.deadline-item--released {
  border-color: rgb(var(--v-theme-info));
  background: rgba(var(--v-theme-info), 0.1);
}

.deadline-status {
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.04em;
  text-transform: uppercase;
}

.deadline-item--due .deadline-status {
  color: rgb(var(--v-theme-error));
}

.deadline-item--released .deadline-status {
  color: rgb(var(--v-theme-info));
}
</style>

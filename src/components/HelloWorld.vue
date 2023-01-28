<template>
<h2 id="nextmsg">{{ nextmsg }}</h2>
<h2>{{ msg }}</h2>
</template>

<script setup>
import { onMounted, ref } from 'vue'
/* ---------------- props ------------------- */

/* ---------------- data -------------------- */
const tasks = [
  { desc: "Pylon", time: 600, keys: ["b", "e"] },
  { desc: "Pylon", time: 600, keys: ["b", "e"] },
  { desc: "Pylon", time: 600, keys: ["b", "e"] },
  { desc: "Nexus", time: 600, keys: ["b", "q"] },
  { desc: "Gateway", time: 800, keys: ["b", "a"] },
  { desc: "Forge", time: 800, keys: ["b", "s"] },
  { desc: "Cybernetix Core", time: 800, keys: ["b", "y"] },
  { desc: "Cannon", time: 800, keys: ["b", "x"] },
  { desc: "Shield Battery", time: 800, keys: ["b", "c"] },
  { desc: "Gas", time: 800, keys: ["b", "w"] },

  { desc: "Twilight", time: 1000, keys: ["g", "q"] },
  { desc: "Stargate", time: 1000, keys: ["g", "w"] },
  { desc: "Robo", time: 1000, keys: ["g", "e"] },
  { desc: "Templar Archives", time: 1000, keys: ["g", "a"] },
  { desc: "Fleet Beacon", time: 1000, keys: ["g", "s"] },
  { desc: "Robo BAY", time: 1000, keys: ["g", "d"] },
  { desc: "DT Shrine", time: 1000, keys: ["g", "y"] },

  { desc: "Warpgate", time: 1000, keys: ["h", "Tab", "e"] },
  // { desc: "Blink-Update", time: 1000, keys: ["h", "Tab", "w"] },
  { desc: "Charge-Update", time: 1000, keys: ["h", "Shift", "Tab", "q"] },
  { desc: "Attack/Def Update", time: 1000, keys: ["h", "Tab", "q", "w"] },
  // { desc: "Storm-Update", time: 1000, keys: ["h", "Shift", "Tab", "q"] },
  // { desc: "DT-Blink-Update", time: 500, keys: ["h", "Shift", "Tab", "y"] },
  // { desc: "Colossus Range-Update", time: 500, keys: ["h", "Shift", "Tab", "e"] },

  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Drones", time: 300, keys: ["h", "q"] },
  { desc: "Stalker", time: 1000, keys: ["n", "e"] },
  { desc: "Adept", time: 1000, keys: ["n", "r"] },
  { desc: "Sentry", time: 1000, keys: ["n", "w"] },
  { desc: "Warpprism", time: 1000, keys: ["n", "w"]},
  { desc: "Immortal", time: 1000, keys: ["n", "e"] },
  { desc: "Disruptor", time: 1000, keys: ["n", "t"] },
  { desc: "Colossus", time: 1000, keys: ["n", "r"] },
  { desc: "Tempest", time: 1000, keys: ["n", "Tab", "r"] },
  { desc: "Carrier", time: 1000, keys: ["n", "Tab", "f"] },
  { desc: "Oracle", time: 1000, keys: ["n", "Tab", "w"] },
  { desc: "Phoenix", time: 1000, keys: ["n", "Tab", "q"] },

  // { desc: "Hallucination", time: 1300, keys: ["4", " ", "c", "f"] },
  // { desc: "Warpprism Siege/Unsiege", time: 1500, keys: ["Dead", "^", "y"] },
  { desc: "Jump To Army", time: 1500, keys: ["4", " "] },
  { desc: "Jump To Army", time: 1500, keys: ["4", " "] },
  { desc: "Jump To Army", time: 1500, keys: ["4", " "] },
  { desc: "Chronoboost", time: 1500, keys: ["h", "a"] },
  { desc: "Recall", time: 1500, keys: ["h", "s"] },
  { desc: "Superbattery", time: 1500, keys: ["h", "d"] },
  { desc: "Nexus -> Group", time: 1500, keys: ["Shift", "H"] },
  { desc: "Twilight -> Group", time: 1500, keys: ["Shift", "H"] },
  { desc: "DT-Shine -> Group", time: 1500, keys: ["Shift", "H"] },
  { desc: "Cybercore -> Group", time: 1500, keys: ["Shift", "H"] },
  { desc: "Forge -> Group", time: 1500, keys: ["Shift", "H"] },
  { desc: "Templar Archives -> Group", time: 1500, keys: ["Shift", "H"] },
  { desc: "Nexus -> Group", time: 1500, keys: ["Shift", "H"] },
  { desc: "Robo -> Group", time: 1500, keys: ["Shift", "N"] },
  { desc: "Robo -> Group", time: 1500, keys: ["Shift", "N"] },
  { desc: "Robo -> Group", time: 1500, keys: ["Shift", "N"] },
  { desc: "Robo -> Group", time: 1500, keys: ["Shift", "N"] },
  { desc: "Robo -> Group", time: 1500, keys: ["Shift", "N"] },
  { desc: "Cancel", time: 1500, keys: ["b"] },
]
/* ---------------- refs -------------------- */
const msg = ref("hello")
const nextmsg = ref("hello")

/* ---------------- computed ---------------- */

/* ---------------- functions --------------- */
let correctKeyCount;
let currentTask;
let nextTask = tasks[0];
function choose() {
  currentTask = nextTask;
  nextTask    = tasks[Math.floor(Math.random() * tasks.length)];
  msg.value   = currentTask.desc;
  nextmsg.value   = nextTask.desc;
  correctKeyCount = 0;
}

function correct() {
  document.body.style.background = "#3c6";
  setTimeout(() => document.body.style.background = "darkslategray", 220)
}

/* ---------------- watchers ---------------- */
onMounted(() => {
  choose();
  
  document.addEventListener('keydown', (e) => {
    console.log(e.key);
    e.preventDefault();

    if(e.key == currentTask.keys[correctKeyCount]) {
      correctKeyCount++;
    } else {
      choose();
    }

    if(correctKeyCount == currentTask.keys.length) {
      correct()
      choose();
    }
  })
})

</script>

<style>
body {
  background: darkslategray;
  transition: all 110ms ease-in-out;
}
h2 {
  color: white;
}
#nextmsg {
  color: #777;
}
</style>
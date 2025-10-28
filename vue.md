# Vue.js Code Examples

## Declare State

```vue
<script setup>
import { ref } from "vue";
const name = ref("John");
</script>

<template>
  <h1>Hello {{ name }}</h1>
</template>
```

## Update State

```vue
<script setup>
import { ref } from "vue";
const name = ref("John");
name.value = "Jane";
</script>

<template>
  <h1>Hello {{ name }}</h1>
</template>
```

## Computed State

```vue
<script setup>
import { ref, computed } from "vue";
const count = ref(10);
const doubleCount = computed(() => count.value * 2);
</script>

<template>
  <div>{{ doubleCount }}</div>
</template>
```

## Minimal Template

```vue
<script setup>
import { ref } from "vue";
const name = ref("John");
</script>

<template>
  <h1>Hello {{ name }}</h1>
</template>
```

## Styling

### Component File

```vue
<template>
  <h1 class="title">I am red</h1>
  <button style="font-size: 10rem">I am a button</button>
</template>

<style scoped>
.title {
  color: red;
}
</style>
```

## Looping

```vue
<script setup>
const colors = ["red", "green", "blue"];
</script>

<template>
  <ul>
    <li
      v-for="color in colors"
      :key="color"
    >
      {{ color }}
    </li>
  </ul>
</template>
```

## Event Handling

```vue
<script setup>
import { ref } from "vue";
const count = ref(0);

function incrementCount() {
  count.value++;
}
</script>

<template>
  <p>Counter: {{ count }}</p>
  <button @click="incrementCount">+1</button>
</template>
```

## DOM Ref

```vue
<script setup>
import { useTemplateRef, onMounted } from "vue";

const inputElement = useTemplateRef("inputElement");

onMounted(() => {
  inputElement.value.focus();
});
</script>

<template>
  <input ref="inputElement">
</template>
```

## Conditional Rendering

```vue
<script setup>
import { ref, computed } from "vue";
const TRAFFIC_LIGHTS = ["red", "orange", "green"];
const lightIndex = ref(0);

const light = computed(() => TRAFFIC_LIGHTS[lightIndex.value]);

function nextLight() {
  lightIndex.value = (lightIndex.value + 1) % TRAFFIC_LIGHTS.length;
}
</script>

<template>
  <button @click="nextLight">Next light</button>
  <p>Light is: {{ light }}</p>
  <p>
    You must
    <span v-if="light === 'red'">STOP</span>
    <span v-else-if="light === 'orange'">SLOW DOWN</span>
    <span v-else-if="light === 'green'">GO</span>
  </p>
</template>
```

## Lifecycle: On Mount

```vue
<script setup>
import { ref, onMounted } from "vue";
const pageTitle = ref("");

onMounted(() => {
  pageTitle.value = document.title;
});
</script>

<template>
  <p>Page title: {{ pageTitle }}</p>
</template>
```

## Lifecycle: On Unmount

```vue
<script setup>
import { ref, onUnmounted } from "vue";

const time = ref(new Date().toLocaleTimeString());

const timer = setInterval(() => {
  time.value = new Date().toLocaleTimeString();
}, 1000);

onUnmounted(() => {
  clearInterval(timer);
});
</script>

<template>
  <p>Current time: {{ time }}</p>
</template>
```

## Props

### Parent Component (App.vue)

```vue
<script setup>
import UserProfile from "./UserProfile.vue";
</script>

<template>
  <UserProfile
    name="John"
    :age="20"
    :favourite-colors="['green', 'blue', 'red']"
    is-available
  />
</template>
```

### Child Component (UserProfile.vue)

```vue
<script setup>
const props = defineProps({
  name: {
    type: String,
    required: true,
    default: "",
  },
  age: {
    type: Number,
    required: true,
    default: null,
  },
  favouriteColors: {
    type: Array,
    required: true,
    default: () => [],
  },
  isAvailable: {
    type: Boolean,
    required: true,
    default: false,
  },
});
</script>

<template>
  <p>My name is {{ props.name }}!</p>
  <p>My age is {{ props.age }}!</p>
  <p>My favourite colors are {{ props.favouriteColors.join(", ") }}!</p>
  <p>I am {{ props.isAvailable ? "available" : "not available" }}!</p>
</template>
```

## Emit to Parent

### Parent Component (App.vue)

```vue
<script setup>
import { ref } from "vue";
import AnswerButton from "./AnswerButton.vue";

let isHappy = ref(true);

function onAnswerNo() {
  isHappy.value = false;
}

function onAnswerYes() {
  isHappy.value = true;
}
</script>

<template>
  <p>Are you happy?</p>
  <AnswerButton
    @yes="onAnswerYes"
    @no="onAnswerNo"
  />
  <p style="font-size: 50px">
    {{ isHappy ? "😊" : "😢" }}
  </p>
</template>
```

### Child Component (AnswerButton.vue)

```vue
<script setup>
const emit = defineEmits(["yes", "no"]);

function clickYes() {
  emit("yes");
}

function clickNo() {
  emit("no");
}
</script>

<template>
  <button @click="clickYes">YES</button>
  <button @click="clickNo">NO</button>
</template>
```

## Slots

### Parent Component (App.vue)

```vue
<script setup>
import FunnyButton from "./FunnyButton.vue";
</script>

<template>
  <FunnyButton> Click me! </FunnyButton>
</template>
```

### Child Component (FunnyButton.vue)

```vue
<template>
  <button
    style="
      background: rgba(0, 0, 0, 0.4);
      color: #fff;
      padding: 10px 20px;
      font-size: 30px;
      border: 2px solid #fff;
      margin: 8px;
      transform: scale(0.9);
      box-shadow: 4px 4px rgba(0, 0, 0, 0.4);
      transition: transform 0.2s cubic-bezier(0.34, 1.65, 0.88, 0.925) 0s;
      outline: 0;
    "
  >
    <slot />
  </button>
</template>
```

## Context

### Parent Component (App.vue)

```vue
<script setup>
import { ref, provide } from "vue";
import UserProfile from "./UserProfile.vue";

const user = ref({
  id: 1,
  username: "unicorn42",
  email: "unicorn42@example.com",
});

function updateUsername(username) {
  user.value.username = username;
}

provide("user", { user, updateUsername });
</script>

<template>
  <h1>Welcome back, {{ user.username }}</h1>
  <UserProfile />
</template>
```

### Child Component (UserProfile.vue)

```vue
<script setup>
import { inject } from "vue";
const { user, updateUsername } = inject("user");
</script>

<template>
  <div>
    <h2>My Profile</h2>
    <p>Username: {{ user.username }}</p>
    <p>Email: {{ user.email }}</p>
    <button @click="() => updateUsername('Jane')">
      Update username to Jane
    </button>
  </div>
</template>
```

## Form Input

```vue
<script setup>
import { ref } from "vue";
const text = ref("Hello World");
</script>

<template>
  <p>{{ text }}</p>
  <input v-model="text">
</template>
```

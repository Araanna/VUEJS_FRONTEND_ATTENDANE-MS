<script setup>
import { ref, onMounted } from 'vue';

// Reactive variable to hold the current theme
const currentTheme = ref('lemonade'); // Default to lemonade

// Function to change the theme
const changeTheme = (themeName) => {
  if (typeof document !== 'undefined') {
    document.documentElement.setAttribute('data-theme', themeName);
    currentTheme.value = themeName;
    localStorage.setItem('daisyui-theme', themeName); // Save to local storage
  }
};

// On component mount, check for a saved theme in local storage
onMounted(() => {
  if (typeof localStorage !== 'undefined') {
    const savedTheme = localStorage.getItem('daisyui-theme');
    if (savedTheme) {
      changeTheme(savedTheme); // Apply the saved theme
    } else {
      // If no theme is saved, check system preference or apply default
      const prefersDark = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
      if (prefersDark) {
        changeTheme('green'); // Apply green if system prefers dark
      } else {
        changeTheme('lemonade'); // Apply lemonade if system prefers light or no preference
      }
    }
  }
});
</script>

<template>
  <div class="min-h-screen bg-base-100 text-base-content flex flex-col items-center justify-center p-4">
    <div class="card w-full max-w-lg shadow-xl bg-base-200 p-6 rounded-box">
      <h1 class="text-3xl font-bold mb-4 text-center">DaisyUI Theme Switcher 🎨</h1>

      <div class="form-control mb-6">
        <label class="label cursor-pointer">
          <span class="label-text text-lg">Current Theme: <span class="font-bold capitalize">{{ currentTheme }}</span></span>
          <input
            type="checkbox"
            class="toggle toggle-lg toggle-primary"
            :checked="currentTheme === 'green'"
            @change="currentTheme === 'green' ? changeTheme('lemonade') : changeTheme('green')"
          />
        </label>
        <p class="text-sm text-center mt-2">Toggle to switch between Lemonade (light) and Green (dark).</p>
      </div>

      <div class="divider">Components in action</div>

      <div class="space-y-4">
        <button class="btn btn-primary w-full">Primary Button</button>
        <button class="btn btn-secondary w-full">Secondary Button</button>
        <button class="btn btn-accent w-full">Accent Button</button>
        <button class="btn btn-neutral w-full">Neutral Button</button>

        <div class="alert alert-info shadow-lg">
          <div>
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="stroke-current flex-shrink-0 w-6 h-6"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
            <span>Info: This is an info alert!</span>
          </div>
        </div>

        <div class="card bg-base-300 shadow-xl">
          <div class="card-body">
            <h2 class="card-title">A DaisyUI Card</h2>
            <p>This card's background is `bg-base-300`.</p>
            <div class="card-actions justify-end">
              <button class="btn btn-success">Success</button>
            </div>
          </div>
        </div>

        <div class="flex flex-col gap-2">
          <input type="text" placeholder="Type here" class="input input-bordered w-full" />
          <select class="select select-bordered w-full">
            <option disabled selected>Select an option</option>
            <option>Option 1</option>
            <option>Option 2</option>
          </select>
        </div>
      </div>
    </div>
  </div>
</template>
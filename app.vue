<template>
  <div>
    <h1>{{ name }}</h1>
    <ul>
      <li v-for="project in projects" :key="project.name">
        <h2>{{ project.name }}</h2>
        <p>{{ project.description }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup>
const name = 'RUKSHAN JS';
const projects = [
  {
    name: 'Project 1',
    description: 'This is project 1',
  },
  {
    name: 'Project 2',
    description: 'This is project 2',
  },
];
</script>

<style>
</style>

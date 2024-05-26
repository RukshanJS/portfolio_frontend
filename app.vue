<template>
  <h1>{{ name }}</h1>
  <div>
    <h1>PROJECTS</h1>
    <div v-if="pending">Loading...</div>
    <ul>
      <li v-for="project in projects" :key="project.name">
        <h2>{{ project.name }}</h2>
        <p>{{ project.description }}</p>
      </li>
    </ul>
  </div>

  <div>
    <h1>BLOGS</h1>
    <div v-if="pending2">Loading...</div>
    <ul>
      <li v-for="blog in blogs" :key="blog.title">
        <h2>{{ blog.title }}</h2>
        <p>{{ blog.content }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup>
const name = 'RUKSHAN JS';
const { data: projects, pending, error } = useFetch('http://localhost:5000/projects');
const { data: blogs, pending2, error2 } = useFetch('http://localhost:5000/blogs');
</script>

<style></style>

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
  <div>
    <h1>Create a New Project</h1>
    <form @submit.prevent="createProject">
      <div>
        <label for="name">Project Name:</label>
        <input type="text" v-model="newProject.name" id="name" required />
      </div>
      <div>
        <label for="description">Project Description:</label>
        <textarea v-model="newProject.description" id="description" required></textarea>
      </div>
      <button type="submit">Create Project</button>
    </form>
  </div>
</template>

<script setup>
const name = 'RUKSHAN JS';
const { data: projects, pending, error } = useFetch('http://localhost:5000/projects');
const { data: blogs, pending2, error2 } = useFetch('http://localhost:5000/blogs');

const newProject = ref({
  name: '',
  description: '',
});

const createProject = async () => {
  console.log(newProject.value);

  try {
    const response = await fetch('http://localhost:5000/projects', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(newProject.value),
    });

    if (!response.ok) {
      throw new Error('Failed to create project');
    }

    const result = await response.json();
    projects.value.push(result); // Add the new project to the list

    // Clear the form
    newProject.value.name = '';
    newProject.value.description = '';
  } catch (error) {
    console.error('Error:', error);
  }
};
</script>

<style></style>

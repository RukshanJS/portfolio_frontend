<template>
  <h1>{{ name }}</h1>
  <div>
    <h1>PROJECTS</h1>
    <div v-if="pending">Loading...</div>
    <ul>
      <li v-for="project in projects" :key="project.name">
        <h2>{{ project._id }}</h2>
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
  <div>
    <h1>Dlete a New Project</h1>
    <form @submit.prevent="deleteProject">
      <div>
        <label for="name">Project Name:</label>
        <input type="text" v-model="deleteProjectForm.id" id="name" required />
        <button type="submit">Delete Project</button>
      </div>
    </form>
  </div>
  <div>
    <h1>Update a Project</h1>
    <form @submit.prevent="updateProject">
      <div>
        <label for="id">Project Id:</label>
        <input type="text" v-model="updateProjectForm.id" id="id" required />
      </div>
      <div>
        <label for="updateName">Project Name:</label>
        <input type="text" v-model="updateProjectForm.name" id="updateName" required />
      </div>
      <div>
        <label for="updateDescription">Project Description:</label>
        <textarea v-model="updateProjectForm.description" id="updateDescription" required></textarea>
      </div>
      <button type="submit">Update Project</button>
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

const updateProjectForm = ref({
  id: '',
  name: '',
  description: '',
});

const deleteProjectForm = ref({
  id: '',
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

// Update project by id by using patch
const updateProject = async () => {
  console.log(newProject.value);

  try {
    const response = await fetch(`http://localhost:5000/projects/${updateProjectForm.value.id}`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(updateProjectForm.value),
    });

    if (!response.ok) {
      throw new Error('Failed to update project');
    }

    const result = await response.json();
    // Update the project in the local list
    const index = projects.value.findIndex((project) => project._id === updateProjectForm.value.id);
    if (index !== -1) {
      projects.value[index] = result;
    }

    // Clear the form
    newProject.value.name = '';
    newProject.value.description = '';
  } catch (error) {
    console.error('Error:', error);
  }
};

const deleteProject = async () => {
  console.log(deleteProjectForm.value);

  try {
    const response = await fetch(`http://localhost:5000/projects/${deleteProjectForm.value.id}`, {
      method: 'DELETE',
    });

    if (!response.ok) {
      throw new Error('Failed to delete project');
    }

    const result = await response.json();
    // Update the project in the local list
    const index = projects.value.findIndex((project) => project._id === deleteProjectForm.value.id);
    if (index !== -1) {
      projects.value.splice(index, 1);
    }

    // Clear the form
    newProject.value.name = '';
    newProject.value.description = '';
  } catch (error) {
    console.error('Error:', error);
  }
};
</script>

<style></style>

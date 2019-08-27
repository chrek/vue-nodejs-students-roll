<template>
  <div id="app" class="small-container">
    <h1>Students</h1>
    <student-form @add:student="addStudent" />
    
    <!-- <student-table v-bind:students="students" /> -->
    <student-table 
      :students="students" 
      @delete:student="deleteStudent" 
      @edit:student="editStudent" 
    />
  </div>
</template>
<script>
import StudentForm from './components/StudentForm';
import StudentTable from './components/StudentTable';

export default {
  name: "App",
  components: {
    StudentTable,
    StudentForm,    
  },
  data() {
    return {
      students: [],
    }
  },

  mounted() {
    this.getStudents()
  },

  methods: {
    async getStudents() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users')
        const data = await response.json()
        this.students = data
      } catch (error) {
        console.error(error)
      }
    },

    async addStudent(student) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST', 
          body: JSON.stringify(student), 
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        })
        const data = await response.json()
        this.students = [...this.students, data]
      } catch (error) {
        console.error(error)
      }
    }, 
    async deleteStudent(id) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'DELETE'
        });
        this.students = this.students.filter(
          student => student.id !== id);
      } catch (error) {
        console.error(error)
      }
    },
    async editStudent(id, updatedStudent) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT', 
          body: JSON.stringify(updatedStudent), 
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        })
        const data = await response.json()
        this.students = this.students.map(
          student => (student.id === id ? data : student)
        )
      } catch (error) {
        console.error(error)
      }
    }
  },
};
</script>
<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 980px;
  }
</style>

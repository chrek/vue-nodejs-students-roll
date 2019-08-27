<template>
  <div id="student-table">
    <p v-if="students.length < 1" class="empty-table">
      No students
    </p>
    <table id="students">
      <thead>
        <tr>
          <th>Student Name</th>
          <th>Student Email</th>
          <th>Web Site</th>
          <th>Phone</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="student in students" :key="student.id">
          <td v-if="editing === student.id">
            <input class="edit" type="text" v-model="student.name" />
          </td>
          <td v-else>{{ student.name }}</td>
          <td v-if="editing === student.id">
            <input class="edit" type="text" v-model="student.email" />
          </td> 
          <td v-else>{{ student.email }}</td>
          <td v-if="editing === student.id">
            <input class="edit" type="text" v-model="student.website" />
          </td>
          <td v-else>{{ student.website }}</td>

          <td v-if="editing === student.id">
            <input class="edit" type="text" v-model="student.phone" />
          </td>
          <td v-else>{{ student.phone }}</td>

          <td v-if="editing === student.id" class="group">
            <button @click="editStudent(student)">Save</button>
            <button class="muted-button" @click="cancelEdit(student)">Cancel</button>
          </td>
          <td v-else class="group">
            <!-- <button @click="editMode(student.id)">Edit</button> -->
            <button @click="editMode(student)">Edit</button>
            <button @click="$emit('delete:student', student.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>    
  </div>
</template>

<script>
  export default {
    name: 'student-table',
    props: {
      students: Array,      
    },
    data() {
      return {
        editing: null,
      }
    },
    methods: {
      /* editMode(id) {
        this.editing = id
      }, */
      editMode(student) {
        this.cachedStudent = Object.assign({}, student)
        this.editing = student.id
      },
      cancelEdit(student) {
        Object.assign(student, this.cachedStudent)
        this.editing = null;
      },
      editStudent(student) {
        if (student.name === '' || student.email === '' || student.country === '' || student.phone === '') {
          return
        }
        this.$emit('edit:student', student.id, student)
        this.editing = null
      },
    },
  }
</script>

<style scoped>
/*   button {
    margin: 0 0.5rem 0 0;
  } */
  button {
    width: 47%;
    /* display: inline-block; */
    background-color: #4CAF50;
    color: white;
    /* padding: 14px 20px; */
    padding: 18px 15px;
    /* margin: 8px 0; */
    margin: 8px 2px 8px 1px;
    /* margin: 0 0.5rem 0.5rem 0; */
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .edit {
    width: 100%;
  }

  /* #students button #btnSave, #students button #btnCancel {
    width: 45%;
    background-color: #4CAF50;
    color: white;
    padding: 14px 20px;
    margin: 8px 0;
    margin: 0 0.5rem 0 0;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  } */

  div {
    border-radius: 5px;
    background-color: #f2f2f2;
    padding: 20px;
  }
  #students {
    font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }

  #students td, #students th {
    border: 1px solid #ddd;
    padding: 8px;
  }

  #students tr:nth-child(even){background-color: #f2f2f2;}

  #students tr:hover {background-color: #ddd;}

  #students th {
    padding-top: 12px;
    padding-bottom: 12px;
    text-align: left;
    background-color: #4CAF50;
    color: white;
  }  
</style>
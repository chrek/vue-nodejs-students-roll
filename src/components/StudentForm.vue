<template>
  <div id="student-form">
    <form @submit.prevent="handleSubmit">
      <label>Student Name</label>
      <input 
        ref="first"
        type="text" 
        :class="{ 'has-error': submitting && invalidName }"
        v-model="student.name" 
        @focus="clearStatus" 
        @keypress="clearStatus"
      />
      <label>Student Email</label>
      <input 
        type="text" 
        :class="{ 'has-error': submitting && invalidEmail }"
        v-model="student.email" 
        @focus="clearStatus" 
      />
      <label>Web Site</label>
      <input 
        type="text" 
        :class="{ 'has-error': submitting && invalidWebSite }"
        v-model="student.website" 
        @focus="clearStatus"
      />
      <label>Phone</label>
      <input 
        type="text" 
        :class="{ 'has-error': submitting && invalidPhone }"
        v-model="student.phone" 
        @focus="clearStatus" 
      />

      <p v-if="error && submitting" class="error-message">
        ! Please provide values for required fields
      </p>
      <p v-if="success" class="success-message">
        Student successfully added
      </p>       
      <button>Add Student</button>      
    </form>    
  </div>
</template>

<script>
export default {
  name: "student-form",
  data() {
    return {
      //submitting state
      submitting: false,
      error: false,
      success: false,
      //student data property
      student: {
        name: '',
        email: '',
        website: '',
        phone: '',
      },
    }
  },
  methods: {
    handleSubmit() {
      this.submitting = true
      this.clearStatus()

      if (this.invalidName || this.invalidEmail || this.invalidWebSite || this.invalidPhone) {
        this.error = true
        return
      }
      //console.log('Testing the custom method handleSubmit()')
      this.$emit('add:student', this.student)
      this.$refs.first.focus()
      this.student = {
        name: '',
        email: '',
        website: '',
        phone: '',
      }
      this.error = false
      this.success = true
      this.submitting = false
    },

    clearStatus() {
      this.success = false
      this.error = false
    }
  },
  computed: {
    invalidName() {
      return this.student.name === ''
    },
    invalidEmail() {
      return this.student.email === ''
    },
    invalidWebSite() {
      return this.student.website === ''
    },
    invalidPhone() {
      return this.student.phone === ''
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  form {
    margin-bottom: 2rem;
  }

  [class*='-message'] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }

  .has-error {
    border: 1px solid #eee;
    border-color: red;
  }

  div {
    border-radius: 5px;
    background-color: #f2f2f2;
    padding: 20px;
  }
  input {
    width: 100%;
    padding: 12px 20px;
    margin: 8px 0;
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }

  button {
    width: 25%;
    background-color: #4CAF50;
    color: white;
    padding: 14px 20px;
    margin: 8px 0;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  
</style>
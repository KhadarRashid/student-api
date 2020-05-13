<template>
  <div id="app">
  <NewStudentForm v-on:student-added="newStudentAdded"></NewStudentForm>

  <StudentTable v-bind:students="students"
  v-on:student-present="studentArrivedOrLeft"
  v-on:delete-student="studentDeleted">
  </StudentTable>

  <StudentMessage v-bind:message="message" v-bind:name="name"></StudentMessage>
  </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentMessage from './components/StudentMessage.vue'
import StudentTable from './components/StudentTable.vue'

export default {
  name: 'App',
  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable
  },
  mounted(){
    this.updateStudents()
  },
  data() {
    return{
      students: [],
      message:'',
      name:''
      
    }
  },

  methods: {
    newStudentAdded(student) {
      this.$student_api.addStudent(student).then( student => {
        this.updateStudents()
      }).catch( err => {
        let message = err.response.data.join(",")
        alert("Error adding student, ", message)
      })
    }, 
    studentArrivedOrLeft(student) {
      this.$student_api.updateStudents(student).then( () => { 
        // this is causing (         ^)issues where i don't understand. You wrote updateStudent even though 
        // i can't find anywhere where you say the singular form again and even if i change it to it it doesn't work, 
        // I'll throw in the towel as this lab has eaten a lot of my time (it was my bad too since i fell behin in class)
        this.name = student.name
        this.message = student.present ? 'Welcome, ' : 'Goodbye, '
        this.updateStudents()
      })
    },
    studentDeleted(student) {
      this.$student_api.deleteStudent(student).then( () => {
        this.updateStudents()
      })
    },
    updateStudents() {
      this.$student_api.getAllStudents().then( students => {
        this.students = students
      })
    }
  }
}
</script>

<style>

</style>
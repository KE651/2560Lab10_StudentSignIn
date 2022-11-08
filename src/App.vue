<!-- keeps list of students, provides list to StudentTable -->
<template>
  <div id="app">
    <new-student-form v-on:student-added="newStudentAdded"></new-student-form>
    <student-table
        v-bind:students="students"
        v-on:student-arrived-or-left="studentArrivedOrLeft"
        v-on:delete-student="studentDeleted">
    </student-table>
    <student-message v-bind:student="mostRecentStudent"></student-message>
  </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentTable from './components/StudentTable.vue'
import StudentMessage from './components/StudentMessage.vue'
import StudentRow from './components/StudentRow.vue'

export default {
  name: "App",
  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable
  }, // end components
  
  data() {
    return {
      students: [], // students is an array of objects {name, starID}
      mostRecentStudent: {}
    } // end return
  }, // end data

  methods: {
    newStudentAdded(student) {
      // console.log('App.vue student: ', student)
      this.students.push(student)
      this.students.sort(function(s1, s2) {
        return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1
      }) // end sort
    }, // end newStudentAdded

    studentArrivedOrLeft(student,present) {
      // find student in this.students and set present value
      // find will return undefined if no match
      let updateStudent= this.students.find( function(s) {
        if (s.name == student.name && s.starID == student.starID) {
          return true } // end if
      }) // end updateStudent

      if (updateStudent) {
        student.present = present
        this.mostRecentStudent = student
      } // end updateStudent
    }, // end studentArrivedOrLeft

    studentDeleted(student) {
      this.students = this.students.filter( function(s) {
        if (s!= student) {
          return true
          // filter returns new array: all students for whom function returns True
          // another option would be using splice function but need index in array
        } // end if
      }) // end filter

      this.mostRecentStudent = {} // clear welcome/goodbye message when student deleted
    } // end studentDeleted

  } // end methods
} // end export
</script>

<style>
@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css";
</style>
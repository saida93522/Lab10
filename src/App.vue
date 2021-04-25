<template>
  <div id="app">
    <new-student-form v-on:student-added="newStudentAdded"></new-student-form>
    <student-table
      v-bind:students="students"
      v-on:student-present="studentArrivedOrLeft"
      v-on:delete-student="studentDeleted"
    ></student-table>
    <student-message v-bind:student="mostRecentStudent"> </student-message>
  </div>
</template>

<script>
import NewStudentForm from "./components/NewStudentForm.vue";
import StudentTable from "./components/StudentTable.vue";
import StudentMessage from "./components/StudentMessage.vue";

export default {
  name: "App",
  emits: ["student-added"], // Document the events this component emits
  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable,
  },
  data() {
    return {
      students: [],
      mostRecentStudent: {},
    };
  },
  methods: {
    newStudentAdded(student) {
      this.students.push(student);
      this.students.sort(function (s1, s2) {
        return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1;
      });
    },
    studentArrivedOrLeft(student, present) {
      let updateStudent = this.students.find((s) => {
        if (s.name == student.name && s.starID == student.starID) {
          return true;
        }
      });
      if (updateStudent) {
        updateStudent.present = present;
        this.mostRecentStudent = student;
      }
    },
    studentDeleted(student) {
      this.students = this.students.filter((s) => {
        s != student;
      });
      this.mostRecentStudent = {}; //clear message
    },
  },
};

//
</script>

<style>
@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

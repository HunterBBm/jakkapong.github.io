<template>
  <div id="view-employee">
    <ul class="collection with-header">
      <li class="collection-header"><h4>{{name}} {{lastname}}</h4></li>
      <li class="collection-item">Employee ID#: {{employee_id}}</li>
      <li class="collection-item">เบอร์โทร: {{phone}}</li>
      <li class="collection-item">อายุ: {{age}}</li>
      <li class="collection-item">ที่อยู่: {{address}}</li>
    </ul>
    <router-link to="/" class="btn grey">Back</router-link>
    <button @click="deleteEmployee" class="btn red">Delete</button>

    <div class="fixed-action-btn">
      <router-link v-bind:to="{ name: 'edit-employee', params: { employee_id: employee_id }}" class="btn-floating btn-large red">
        <i class="fa fa-pencil"></i>
      </router-link>
    </div>
  </div>
</template>

<script>
import db from './firebaseInit';
export default {
  name: 'view-employee',
  data() {
    return {
      employee_id: null,
      name: null,
      lastname: null,
      phone: null,
      age: null,
      address: null
    };
  },
  beforeRouteEnter(to, from, next) {
    db
      .collection('employees')
      .where('employee_id', '==', to.params.employee_id)
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          next(vm => {
            vm.employee_id = doc.data().employee_id;
            vm.name = doc.data().name;
            vm.lastname = doc.data().lastname;
            vm.phone = doc.data().phone;
            vm.age = doc.data().age;
            vm.address = doc.data().address;
          });
        });
      });
  },
  watch: {
    $route: 'fetchData'
  },
  methods: {
    fetchData() {
      db
        .collection('employees')
        .where('employee_id', '==', this.$route.params.employee_id)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            this.employee_id = doc.data().employee_id;
            this.name = doc.data().name;
            this.lastname = doc.data().lastname;
            this.phone = doc.data().phone;
            this.age = doc.data().age;
            this.address = doc.data().address;
          });
        });
    },
    deleteEmployee() {
      if (confirm('คุณแน่ใจว่าจะลบข้อมูลนี้')) {
        db
          .collection('employees')
          .where('employee_id', '==', this.$route.params.employee_id)
          .get()
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
              doc.ref.delete();
              this.$router.push('/');
            });
          });
      }
    }
  }
};
</script>

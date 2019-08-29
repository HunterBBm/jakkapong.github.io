<template>
  <div id="home">
    <ul class="collection with-header">
      <li class="collection-header"><h4>รายชื่อสมาชิก</h4></li>
      <li v-for="employee in employees" v-bind:key="employee.id" class="collection-item">
        {{employee.employee_id}}: {{employee.name}} {{employee.lastname}} 
        <div>เบอร์โทร :{{employee.phone}} </div>
         <router-link class="secondary-content" v-bind:to="{ name: 'view-employee', params: { employee_id: employee.employee_id }}"><i class="fa fa-eye"></i></router-link>
        <div>อายุ :{{employee.age}} </div>
        <div>ที่อยู่ :{{employee.address}}</div>
      </li>
    </ul>
    <div class="fixed-action-btn">
      <router-link to="/new" class="btn-floating btn-large red">
        <i class="fa fa-plus"></i>
      </router-link>
    </div>
  </div>
</template>

<script>
import db from './firebaseInit';
export default {
  name: 'home',
  data() {
    return {
      employees: [],
      loading: true
    };
  },
  created() {
    db
      .collection('employees')
      .orderBy('employee_id')
      .get()
      .then(querySnapshot => {
        this.loading = false;
        querySnapshot.forEach(doc => {
          console.log(doc.data())
          const data = {
            id: doc.id,

            employee_id: doc.data().employee_id,
            name: doc.data().name,
            lastname: doc.data().lastname,
            phone: doc.data().phone,
            age: doc.data().age,
            address: doc.data().address,
          };
          this.employees.push(data);
        });
      });
  }
};
</script>
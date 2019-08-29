<template>
  <div id="new-employee">
    <h3>แก้ไข</h3>
    <div class="row">
    <form @submit.prevent="updateEmployee" class="col s12">
      <div class="row">
        <div class="input-field col s12">
          <input disabled type="text" v-model="employee_id" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="name" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="lastname" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="phone" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="age" required>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="address" required>
        </div>
      </div>
      <button type="submit" class="btn">Submit</button>
      <router-link to="/" class="btn grey">Cancel</router-link>
    </form>
  </div>
  </div>
</template>

<script>
  import db from './firebaseInit'
  export default {
    name: 'edit-employee',
    data () {
      return {
        employee_id: null,
        name: null,
        lastname: null,
        phone: null,
        age: null,
        address: null
      }
    },
    beforeRouteEnter (to, from, next) {
      db.collection('employees').where('employee_id', '==', to.params.employee_id).get().then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          next(vm => {
            vm.employee_id = doc.data().employee_id;
            vm.name = doc.data().name;
            vm.lastname = doc.data().lastname;
            vm.phone = doc.data().phone;
            vm.age = doc.data().age;
            vm.address = doc.data().address;
          })
        })
      })
    },
    watch: {
      '$route': 'fetchData'
    },
    methods: {
      fetchData () {
        db.collection('employees').where('employee_id', '==', this.$route.params.employee_id).get().then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            this.employee_id = doc.data().employee_id;
            this.name = doc.data().name;
            this.lastname = doc.data().lastname;
            this.phone = doc.data().phone;
            this.age = doc.data().age;
            this.address = doc.data().address;
          })
        })
      },
      updateEmployee () {
        if (confirm('คุณต้องการเปลี่ยนแปลงข้อมูล')) {
        db.collection('employees').where('employee_id', '==', this.$route.params.employee_id).get().then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            doc.ref.update({
              employee_id: this.employee_id,
              name: this.name,
              lastname: this.lastname,
              phone: this.phone,
              age: this.age,
              address: this.address
            })
            .then(() => {
              this.$router.push({ name: 'view-employee', params: { employee_id: this.employee_id }})
            });
          })
        })
      }
      }
    }
  }
</script>
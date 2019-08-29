<template>
  <div id="new-employee">
    <h3>เพิ่มสมาชิก</h3>
    <div class="row">
    <form @submit.prevent="saveEmployee" class="col s12">
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="employee_id" required>
          <label>Employee ID#</label>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="name" required>
          <label>Name</label>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="lastname" required>
          <label>Lastname</label>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="phone" required>
          <label>เบอร์โทร</label>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="age" required>
          <label>อายุ</label>
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input type="text" v-model="address" required>
          <label>ที่อยู่</label>
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
      name: 'new-employee',
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
      methods: {
        saveEmployee () {
          db.collection('employees').add({
              employee_id: this.employee_id,
              name: this.name,
              lastname: this.lastname,
              phone: this.phone,
              age: this.age,
              address: this.address
          })
          .then(docRef => {
            console.log('Client added: ', docRef.id)
            this.$router.push('/')
          })
          .catch(error => {
            console.error('Error adding employee: ', error)
          })
        }
      }
    }
</script>

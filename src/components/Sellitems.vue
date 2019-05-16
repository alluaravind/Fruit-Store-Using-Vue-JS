<template>
  <div id="shopping-list">
    <router-link to="/" style="float:right">Logout</router-link>
    <br>
    <div class="header">
      <h1>{{ header.toLocaleUpperCase() }}</h1>
      <button
        v-if="state === 'default'" class="btn btn-primary" @click="changeState('edit')"
        >Add Item</button>
        <button v-else class="btn btn-cancel"
      @click="changeState('default')">Cancel Adding Item</button>
    </div>
    <div v-if="state === 'edit'" class="add-item-form">
      <input
        v-model="newItem"
        type="text"
        placeholder="Enter Fruit"
        onkeypress="return (event.charCode > 64 &&
event.charCode < 91) || (event.charCode > 96 && event.charCode < 123)"
        style="color:blue"
      >
      <br>
      <br>
      <!-- @keyup.enter="saveItem" -->
      <!-- <p >{{ characterCount }}/50</p><br> -->
      <input
        v-model="newItem1"
        type="text"
        placeholder="Enter Quantity"
        v-on:keypress="isNumber(event)"
        style="color:blue"
      >
      <input
        v-model="newItem2"
        type="text"
        placeholder="Enter Price"
        v-on:keypress="isAnyNumber(event)"
        style="color:blue"
      >
      <button
        class="btn btn-primary"
        v-if="newItem.length > 0 &&newItem1 > 0 && newItem2 > 0"
        @click="saveItem"
      >Save Item</button>
    </div>
    <ul v-for="(item,index) in reversedItems" :key="index">
      <center>
        <div id="added-fruits">
          <li
            :class="{strikeout: item.purchased}"
            @click="togglePurchased(item)"
            style="color:#22292F"
          >Fruit Name: {{ item.label }}</li>
          <li style="color:#22292F">
            Fruit Quantity:
            <p style>{{item.label1}}</p>
          </li>
          <li style="color: #22292F">Fruit Price: {{item.label2}}</li>
          <li>
            <button
              class="qty btn-primary"
              v-if="state1 === 'default1'"
              @click="changeState('update')"
            >Update Quantity</button>
          </li>
          <br>
          <div v-if="state === 'update'">
            <input
              type="text"
              id="update"
              placeholder="Update Quantity"
              @keyup="email = $event.target.value"
              style="color:blue"
            >
            <button class="qty btn-primary" @click="update(index,item.label)">Update</button>
          </div>
          <li>
            <button
              class="qty btn-primary"
              v-if="state2 === 'default2'"
              @click="changeState('updateprice')"
            >Update Price</button>
          </li>
          <div v-if="state === 'updateprice'">
            <input
              type="text"
              id="updateprice"
              placeholder="Update Price"
              @keyup="pass1 = $event.target.value"
              style="color:blue"
            >
            <button class="qty btn-primary" @click="updateprice(index,item.label)">Update</button>
          </div>
        </div>
      </center>
    </ul>
    <p v-if="items.length === 0">No Fruits in Your Account.</p>
  </div>
</template>

<script>
module.exports = {
  data() {
    return {
      state: 'default',
      state1: 'default1',
      state2: 'default2',
      header: 'Welcome to the Store',
      newItem: '',
      newItem1: null,
      newItem2: null,
      items: [],
    };
  },
  computed: {
    // characterCount(){
    //     return this.newItem.length;
    // },
    reversedItems() {
      return this.items.slice(0);
    },
  },
  methods: {
    update(ind, lab) {
      const temp = this.email;
      this.lab = lab.toString();

      //   console.log(this.items.indexOf(45, 2))
      if (this.items[ind].label === lab) {
        //   console.log(this.items[0].label,ind);
        // console.log(this.items[ind].label,ind,lab,temp);
        this.items[ind].label1 = temp;
        // console.log(this.email);
        this.email = '';
        // console.log(this.email);
        this.state = 'default1';
        // this.newit='';
        // $("#update").val("");
      }
    },
    updateprice(ind, lab) {
      const temp = this.pass1;
      this.lab = lab.toString();
      //   console.log(this.items.indexOf(45, 2))
      if (this.items[ind].label === lab) {
        //   console.log(this.items[0].label,ind);
        // console.log(this.items[ind].label,ind,lab,temp);
        this.items[ind].label2 = temp;
        this.pass1 = '';
        // console.log(this.pass1);
        this.state = 'default2';
        // console.log(this.pass1);
        // $("#updateprice").val("");
      }
    },
    saveItem() {
      this.items.push({
        label: this.newItem,
        label1: this.newItem1,
        label2: this.newItem2,
        purchased: false,
      });
      this.newItem = '';
      this.newItem1 = null;
      this.newItem2 = null;
    },
    isNumber(evt) {
      this.evt = evt || window.event;
      const charCode = evt.which ? evt.which : evt.keyCode;
      if (charCode > 31 && (charCode < 48 || charCode > 57)) { evt.preventDefault(); }
      return true;
    },
    isAnyNumber(evt) {
      this.evt = evt || window.event;
      const charCode = evt.which ? evt.which : evt.keyCode;
      if (
        charCode > 31 &&
        (charCode < 48 || charCode > 57) &&
        charCode !== 46
      ) {
        evt.preventDefault();
      } else {
        return true;
      }
      return 0;
    },
    changeState(newState) {
      this.state = newState;
      this.newItem = '';
    },
    togglePurchased(item) {
      this.item.purchased = !item.purchased;
    },
  },
};
</script>
<style scoped>
body {
  background: #eff8ff;
  height: 100vh;
  width: 98vw;
  font-family: system-ui, BlinkMacSystemFont, -apple-system, Segoe UI, Roboto,
    Oxygen, Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0;
  padding: 0;
}
#update,
#updateprice {
  margin: 0 0.5rem 0;
  width: 35%;
  border-radius: 3px;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1);
  border: 1px solid #f1f5f8;
  color: #606f7b;
  padding: 0.5rem 0.75rem;
  box-sizing: border-box;
  font-size: 1.3rem;
  letter-spacing: 0.5px;
  margin: 0.5rem 0;
}
.add-item-form,
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.qty {
  width: 30%;
  border: none;
  border-radius: 3px;
  margin: auto 0;
  padding: 0.5rem 0.75rem;
}
input {
  background-color: cornsilk;
}
h1 {
  color: #3d4852;
}

ul {
  list-style: none;
  padding: 0;
}

a {
  color: #6cb2eb;
  font-size: 1.25rem;
  transition: all 0.1s ease-in;
  margin-top: 0.5rem;
  display: block;
}

a:hover {
  color: #3490dc;
}

li,
p {
  display: flex;
  align-items: center;
  line-height: 1.75;
  letter-spacing: 0.5px;
  color: #3d4852;
  font-size: 1.25rem;
  cursor: pointer;
  transition: all 0.1s ease-in;
}

li:hover {
  color: #22292f;
}

li input {
  margin: 0 0.5rem 0;
}
#shopping-list {
  background: #e68c8c;
  padding: 2rem;
  margin: 1rem;
  border-radius: 3px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.12), 0 2px 4px 0 rgba(0, 0, 0, 0.08);
  width: 92%;
  height: 85%;
  /*max-width: 900px;*/
}
#added-fruits {
  background: #fff;
  padding: 2rem;
  margin: 1rem;
  border-radius: 3px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.12), 0 2px 4px 0 rgba(0, 0, 0, 0.08);
  width: 45%;
  height: 35%;
  /*max-width: 900px;*/
}
.add-item-form,
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.add-item-form input {
  width: 70%;
  border-radius: 3px;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1);
  border: 1px solid #f1f5f8;
  color: #606f7b;
  padding: 0.5rem 0.75rem;
  box-sizing: border-box;
  font-size: 1rem;
  letter-spacing: 0.5px;
  margin: 0.5rem 0;
}
.btn {
  width: 20%;
  border: none;
  border-radius: 3px;
  margin: auto 0;
  padding: 0.5rem 0.75rem;
  flex-shrink: 0;
  cursor: pointer;
  /*font-size: .9rem;*/
  font-size: 20px;
  letter-spacing: 0.5px;
  transition: all 0.1s ease-in;
}

.btn[disabled] {
  background: #8795a1;
}

.btn[disabled]:hover {
  background: #606f7b;
}

.btn-primary {
  background: #6cb2eb;
  color: #fff;
}

.btn-primary:hover {
  background: #3490dc;
}

.btn-cancel {
  width: 30%;
  background: #ef5753;
  color: #fff;
}

.btn-cancel:hover {
  background: #e3342f;
  color: #fff;
}

.strikeout {
  text-decoration: line-through;
  color: #b8c2cc;
}

.strikeout:hover {
  color: #8795a1;
}
</style>

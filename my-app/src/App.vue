<template>
  <!-- แสดงผลหน้าเว็บ-->
  <section>
    <img :src="image" :width="size" :height="size" ref="imageEl" /><br />
    <!--
      <form @submit="saveDataForm">
      <label>ป้อนชื่อเล่น :</label>
      <input type="text"  ref="nicknameEl"/>

      <button type="submit">Save</button>
    </form>
-->
    <h1>ชื่อ : {{ getFullName }}</h1>
    <h2>ชื่อเล่น : {{ nickname }}</h2>
    <h2>อายุ : {{ age }} ปี</h2>
    <span>
      <button @click="increment(1)">+</button>
      <button @click.ctrl="decrement(1)">-</button>
    </span>
    <h2>เงินเดือน : {{ salary }} บาท</h2>
    <h2>เงินปี : {{ getIncome }} บาท</h2>
    <h1>ตำแหน่งงาน : {{ getDepartment }}</h1>
    <button @click="addSalary(5000)">เพิ่มเงินเดือน</button>
    <br><br>
    <div>
      <button @click="toggleVisible">{{ isVisible ? 'ซ่อนข้อมูล' : 'ดูข้อมูล' }}</button>
    </div>
    <div v-show="isVisible">
      <p>ที่อยู่ : <span v-html="address"></span></p>
      <p>
        Social :
        <a :href="social" target="_blank">Facebook</a>
      </p>
      <p v-if="hobby.length === 0">ไม่มีงานอดิเรก</p>
      <!-- v-if ตรงเงื่อนไข จะโชว์-->
      <div v-else>
        <!-- v-else ไม่ตรงจะโชว์-->
        <p>งานอดิเรก</p>
        <ul>
          <li v-for="(item, index) in hobby" :key="index">{{ item }}</li>
          <!-- Loop โดยมาเก็บไว้ใน item โดยใช้ index ให้ค่าไม่ซ้ำ-->
        </ul>
      </div>
      <p>ข้อมูลพื้นฐาน :</p>
      <ul>
        <li v-for="(item, key) in general" :key="key">{{ item }}</li>
        <!-- key คือ ดึงมาแต่ละหัวข้อใน Data เหมือนกัน index -->
      </ul>
      <!--<button @click="showData">คลิ๊กเพื่อดูข้อมูล</button>-->
    </div>
  </section>
</template>

<script>
// js ควบคุมการทำงานของ Componnent method, data
export default {
  name: "App",
  data() {
    return {
      fname: "Max",
      lname: "Za",
      nickname: "",
      age: 25,
      address: "<i>นครราชสีมา</i>",
      image: require("@/assets/users.png"),
      social: "https://www.facebook.com/",
      size: 150,
      hobby: ["ออกกําลังกาย", "เล่นเกม", "ฟังเพลง", "อ่านหนังสือ", "เล่นกีฬา"],
      general: { gender: "ชาย", weight: 74, height: 170, status: false },
      isVisible : false,
      salary : 10000,
    };
  },
  methods: { // methods จะทำงานใหม่ทุกครั้งที่มีการเรียกใช้ รวมถึงการสร้าง function ที่ต้องส่งค่ามา
    // สร้าง method เพื่อเรียกใช้จาก Data แต่ต้องอยู่ใน script เดียวกัน

    showData() {
      alert(`${this.fname + " " + this.lname}`);
    },
    increment(value) {
      this.age += value; //this ข้อมูลdata ที่สร้าง  += data คือการบวกแบบมีค่าให้บวก
    },
    decrement(value) {
      this.age -= value;
    },
    saveDataForm(e) {
      e.preventDefault();
      alert("Save Data Success");
      this.nickname = this.$refs.nicknameEl.value;
    },
    toggleVisible(){
      this.isVisible = !this.isVisible;
    },
    addSalary(value){
      this.salary += value
    }
  },
  computed:{ // computed จะทํางานแค่ครั้งเดียวที่มีการเรียกใช้ แล้วเก็บเป็นรูปแบบ cache เหมาะแก้กับรับค่าที่รออัพเดท
      getFullName() {
      return `${this.fname + " " + this.lname}`;
    },
    getIncome(){
      return this.salary * 12
    },
    getDepartment(){
      return this.salary >= 35000 ? 'PM' : 'DEV' 
    }
    },
    watch: { // ติดตามค่าที่เปลี่ยน หรือข้อมูลที่สนใจ
      salary(value){ 
        if(value > 50000){
          alert("เงินเดือนไม่ควรเกิน 50000 บาท")
          setTimeout(() =>
          this.salary = 20000,
          1000)
        }
      }
    }
};
</script>

<style></style>

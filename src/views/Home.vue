<template>
  <h1 class="title">
    <strong>Jelentkezés szoftverfejlesztői állására </strong>
  </h1>
  <div class="form">
    <div class="row">
      <div class="col-75">
        <div class="container">
          <form ref="jobForm" @submit="submitForm">
            <div class="row">
              <div class="col-50">
                <h3>Személyes adatok</h3>
                <label for="name">
                  <i class="fa fa-user"></i> Teljes név
                </label>
                <label for="name" class="error" v-if="nameError">
                  A név nem megfelelő!
                </label>
                <input
                  type="text"
                  v-model.trim="name"
                  @change="checkName"
                  required
                  placeholder="Teszt Elek"
                />

                <label for="email">
                  <i class="fa fa-envelope"></i> Email
                </label>
                <label for="email" class="error" v-if="emailError">
                  Az e-mail cím nem megfelelő!
                </label>
                <input
                  type="email"
                  @change="checkEmail"
                  required
                  v-model.trim="email"
                  placeholder="balogh.norbert92@gmail.com"
                />

                <label for="phone">
                  <i class="fa fa-phone"></i> Telefonszám
                </label>
                <label for="phone" class="error" v-if="phoneError">
                  A megadott telefonszám nem megfelelő formátumú!
                </label>
                <input
                  type="text"
                  @change="checkPhoneNumber"
                  required
                  v-model.trim="phone"
                  placeholder="+36991234567"
                />

                <label for="adr">
                  <i class="fa fa-address-card-o"></i> Cím adatok
                </label>
                <div class="row">
                  <div class="col-25">
                    <label for="zip">Irányítószám</label>
                    <label for="zip" class="error" v-if="zipError">
                      A megadott irányítószám nem megfelelő formátumú!
                    </label>
                    <input
                      type="text"
                      id="zip"
                      name="zip"
                      placeholder="6726"
                      @change="checkZip"
                      required
                      v-model.trim="zip"
                    />
                  </div>
                  <div class="col-50">
                    <label for="city">Város</label>
                    <label for="city" class="error" v-if="cityError">
                      A megadott város nem megfelelő formátumú!
                    </label>
                    <input
                      type="text"
                      id="city"
                      name="city"
                      placeholder="Szeged"
                      @change="checkCity"
                      required
                      v-model.trim="city"
                    />
                  </div>
                </div>

                <div class="row">
                  <div class="col-25">
                    <label for="street">Utca</label>
                    <label for="street" class="error" v-if="streetError">
                      A megadott utca nem megfelelő formátumú!
                    </label>
                    <input
                      type="text"
                      id="street"
                      name="street"
                      placeholder="Római krt."
                      @change="checkStreet"
                      required
                      v-model.trim="street"
                    />
                  </div>
                  <div class="col-50">
                    <label for="houseNumber">Házszám</label>
                    <label for="street" class="error" v-if="houseNumberError">
                      A megadott házszám nem megfelelő formátumú!
                    </label>
                    <input
                      type="text"
                      id="houseNumber"
                      name="houseNumber"
                      placeholder="21B"
                      @change="checkHouseNumber"
                      required
                      v-model.trim="houseNumber"
                    />
                  </div>
                </div>
              </div>

              <div class="col-50">
                <h3>Állás</h3>
                <label for="job-selector">Pozíció </label>
                <label
                  for="job-selector"
                  class="error"
                  v-if="
                    needSelect &&
                    (selectedJob === '' || selectedJob === 'Kérjük válasszon!')
                  "
                >
                  Még nem választottál!
                </label>
                <select
                  id="job-selector"
                  @click="needSelect = true"
                  v-model="selectedJob"
                >
                  <option v-for="job in jobs" :key="job.name" :value="job.name">
                    {{ job.name }}
                  </option>
                </select>

                <label for="fname">Eddigi munkatapasztalatok:</label>

                <textarea
                  id="field"
                  rows="8"
                  cols="49"
                  v-model.trim="experience"
                  @keypress="countChar"
                  maxlength="500"
                ></textarea>
                <label>
                  Hátralévő karakterek száma: {{ availableCharacterSize }}
                </label>
                <label> <span>&nbsp;</span></label>
                <label for="skill">Technológiai jártasság</label>
                <input
                  type="text"
                  id="skill"
                  v-model.trim="skill"
                  placeholder="JAVA; Spring booot; ..."
                  @keydown="addSkill"
                />
                <div class="experience-container">
                  <div
                    v-for="(skill, index) in mySkills"
                    :key="index"
                    :value="skill.name"
                    @click="removeSkill(index)"
                  >
                    {{ skill.name }}
                  </div>
                </div>
              </div>
            </div>

            <input
              v-bind:class="{ btn: valid }"
              type="submit"
              value="Jelentkezés"
              :disabled="!isFormValid"
            />
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Form from "@/components/Form.vue";

const ONLY_LETTERS_REGEX = /^[a-zA-Z]+$/;
const EMAIL_ADDRESS_REGEX = /\S{3,}@\S+\.[a-z0-9]{2,}/;
const PHONE_REGEX = /^[+]{1}[0-9]{11}$/;

const POSTCODE_REGEX = /^[1-9]\d\d\d$/;
const CITY_REGEX = /^[a-zA-Z\s]*$/;
const HOUSE_NUMVER_REGEX = /^[1-9][0-9]{0,3}[a-zA-Z]{0,1}$/;
const MAX_DESCRIPTION_SIZE = 500;

const DEFAULT_SELECTION = "Kérjük válasszon!";
export default {
  name: "Home",

  components: {
    Form,
  },
  data() {
    return {
      jobs: [
        { name: DEFAULT_SELECTION },
        { name: "Backend" },
        { name: "Frontend" },
        { name: "Full stack" },
      ],
      mySkills: [],
      skill: "",
      availableCharacterSize: MAX_DESCRIPTION_SIZE,
      experience: "",
      selectedJob: DEFAULT_SELECTION,
      needSelect: false,
      name: "",
      email: "",
      phone: "",
      zip: "",
      city: "",
      street: "",
      houseNumber: "",

      nameError: false,
      emailError: false,
      phoneError: false,
      zipError: false,
      cityError: false,
      streetError: false,
      houseNumberError: false,

      isFormValid: false,
    };
  },
  methods: {
    checkName() {
      let names = this.name.trim().split(" ");
      this.nameError =
        names.length < 2 ||
        !ONLY_LETTERS_REGEX.test(names[0]) ||
        !ONLY_LETTERS_REGEX.test(names[1]) ||
        names[0].length < 2 ||
        names[1].length < 2;
    },
    checkEmail() {
      this.emailError = !EMAIL_ADDRESS_REGEX.test(this.email);
    },
    checkPhoneNumber() {
      this.phoneError = !PHONE_REGEX.test(this.phone);
    },
    checkZip() {
      this.zipError = !POSTCODE_REGEX.test(this.zip);
    },
    checkCity() {
      this.cityError = !CITY_REGEX.test(this.city);
    },
    checkStreet() {
      this.streetError = this.street === ""
      console.log("street:" + this.streetError)
    },
    checkHouseNumber() {
      this.houseNumberError = !HOUSE_NUMVER_REGEX.test(this.houseNumber);
    },

    countChar() {
      let len = this.experience.length;
      let reminder = MAX_DESCRIPTION_SIZE - 1 - len;
      if (reminder < 0) {
        this.experience = this.experience.substring(
          0,
          MAX_DESCRIPTION_SIZE - 1
        );
      }
      this.availableCharacterSize = reminder >= 0 ? reminder : 0;
    },

    addSkill() {
      if (this.skill.includes(";")) {
        let actSkill = this.skill.split(";")[0];
        this.mySkills.push({ id: this.mySkills.length, name: actSkill });
        this.skill = "";
      }
    },
    removeSkill(index) {
      this.mySkills.splice(index, 1);
    },
    submitForm(){
        alert('Jelenetkezésést továbbítottuk');
         this.$refs.jobForm.reset();
    }
  },
  computed: {
    valid() {
      this.isFormValid =
        !this.nameError &&
        this.name !== "" &&
        !this.emailError &&
        this.email !== "" &&
        !this.phoneError &&
        this.phone !== "" &&
        !this.zipError &&
        this.zip !== "" &&
        !this.cityError &&
        this.city !== "" &&
        !this.houseNumberError &&
        this.houseNumber !== "" &&
        this.selectedJob !== "" &&
        this.selectedJob !== DEFAULT_SELECTION;

console.log("form:"+this.isFormValid);
      return this.isFormValid;
    },
  },
};
</script>
<style scoped>
@import "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css";
.error {
  padding: 0;
  margin: 0;
  color: red;
  font-size: 70%;
}
.error_border {
  border-bottom: 1px solid red;
}
div {
  padding: 4px;
}
input {
  border: none;
  border-bottom: 1px solid #ccc;
}

.experience-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  flex-flow: row wrap;
}

select {
  width: 100%;
  height: 38px;
  padding: 5px 11px;
  margin-bottom: 20px;
}

select:not(.ant-select-customize-input) .ant-select-selector {
  position: relative;
  background-color: #fff;
  border: 1px solid #d9d9d9;
  border-radius: 2px;
  -webkit-transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
  transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
}

body {
  font-family: Arial;
  font-size: 17px;
  padding: 8px;
}

.form {
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
}

* {
  box-sizing: border-box;
}

.row {
  display: -ms-flexbox; /* IE10 */
  display: flex;
  -ms-flex-wrap: wrap; /* IE10 */
  flex-wrap: wrap;
  margin: 0 -16px;
}

.col-25 {
  -ms-flex: 25%; /* IE10 */
  flex: 25%;
}

.col-50 {
  -ms-flex: 50%; /* IE10 */
  flex: 50%;
}

.col-75 {
  -ms-flex: 75%; /* IE10 */
  flex: 75%;
}

.col-25,
.col-50,
.col-75 {
  padding: 0 16px;
}

.container {
  background-color: #f2f2f2;
  padding: 5px 20px 15px 20px;
  border: 1px solid lightgrey;
  border-radius: 3px;
}

input[type="email"],
input[type="text"] {
  width: 100%;
  margin-bottom: 20px;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

label {
  margin-bottom: 10px;
  position: relative;
  text-align: left;
  display: block;
}

.icon-container {
  margin-bottom: 20px;
  padding: 7px 0;
  font-size: 24px;
}

.btn {
  background-color: #04aa6d;
  color: white;
  padding: 12px;
  margin: 10px 0;
  border: none;
  width: 100%;
  border-radius: 3px;
  cursor: pointer;
  font-size: 17px;
}

.btn:hover {
  background-color: #45a049;
}

a {
  color: #2196f3;
}

hr {
  border: 1px solid lightgrey;
}

span.price {
  float: right;
  color: grey;
}

@media (max-width: 800px) {
  .row {
    flex-direction: column;
  }
  .col-25 {
    margin-bottom: 20px;
  }
}
</style>

<template>
  <h1 class="title">
    <strong>Jelentkezés szoftverfejlesztői állására </strong>
  </h1>
  <div class="form">
    <div class="row">
      <div class="col-75">
        <div class="container">
          <form ref="jobForm" @submit.prevent="submitForm">
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
                  v-model.trim="detail.name"
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
                  v-model.trim="detail.email"
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
                  v-model.trim="detail.phone"
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
                      v-model.trim="detail.zip"
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
                      v-model.trim="detail.city"
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
                      v-model.trim="detail.street"
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
                      v-model.trim="detail.houseNumber"
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
                    (detail.selectedJob === '' ||
                      detail.selectedJob === 'Kérjük válasszon!')
                  "
                >
                  Még nem választottál!
                </label>
                <select
                  id="job-selector"
                  @click="needSelect = true"
                  v-model="detail.selectedJob"
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
                  v-model.trim="detail.experience"
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
                    class="skill"
                    v-for="(skill, index) in detail.skills"
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
  <Details v-if="formsubmitted" v-bind:param="detail" />
</template>

<script>
import Details from "@/views/Details.vue";

const ONLY_LETTERS_REGEX = /^[a-zA-Z]+$/;
const EMAIL_ADDRESS_REGEX = /\S{3,}@\S+\.[a-z0-9]{2,}/;
const PHONE_REGEX = /^[+]{1}[0-9]{11}$/;

const POSTCODE_REGEX = /^[1-9]\d\d\d$/;
const CITY_REGEX = /^[a-zA-Záíóöőüűúé\s]*$/;
const HOUSE_NUMVER_REGEX = /^[1-9][0-9]{0,3}[a-zA-Z]{0,1}$/;
const MAX_DESCRIPTION_SIZE = 500;

const DEFAULT_SELECTION = "Kérjük válasszon!";
const availablejobs = [
  { name: DEFAULT_SELECTION },
  { name: "Backend" },
  { name: "Frontend" },
  { name: "Full stack" },
];

export default {
  name: "ApplyForm",
  components: {
    Details,
  },
  data() {
    return {
      result: Object,
      detail: {
        skills: [],
        experience: "",
        selectedJob: DEFAULT_SELECTION,
        name: "",
        email: "",
        phone: "",
        zip: "",
        city: "",
        street: "",
        houseNumber: "",
      },

      jobs: availablejobs,
      skill: "",
      availableCharacterSize: MAX_DESCRIPTION_SIZE,
      needSelect: false,

      nameError: false,
      emailError: false,
      phoneError: false,
      zipError: false,
      cityError: false,
      streetError: false,
      houseNumberError: false,

      isFormValid: false,
      formsubmitted: false,
    };
  },
  methods: {
    checkName() {
      let names = this.detail.name.trim().split(" ");
      this.nameError =
        names.length < 2 ||
        !ONLY_LETTERS_REGEX.test(names[0]) ||
        !ONLY_LETTERS_REGEX.test(names[1]) ||
        names[0].length < 2 ||
        names[1].length < 2;
    },
    checkEmail() {
      this.emailError = !EMAIL_ADDRESS_REGEX.test(this.detail.email);
    },
    checkPhoneNumber() {
      this.phoneError = !PHONE_REGEX.test(this.detail.phone);
    },
    checkZip() {
      this.zipError = !POSTCODE_REGEX.test(this.detail.zip);
    },
    checkCity() {
      this.cityError = !CITY_REGEX.test(this.detail.city);
    },
    checkStreet() {
      this.streetError = this.detail.street === "";
      console.log("street:" + this.detail.streetError);
    },
    checkHouseNumber() {
      this.houseNumberError = !HOUSE_NUMVER_REGEX.test(this.detail.houseNumber);
    },

    countChar() {
      let len = this.detail.experience.length;
      let reminder = MAX_DESCRIPTION_SIZE - 1 - len;
      if (reminder < 0) {
        this.detail.experience = this.detail.experience.substring(
          0,
          MAX_DESCRIPTION_SIZE - 1
        );
      }
      this.availableCharacterSize = reminder >= 0 ? reminder : 0;
    },

    addSkill() {
      if (this.skill.includes(";")) {
        let actSkill = this.skill.split(";")[0];
        if (actSkill.trim() !== "") {
          this.detail.skills.push({
            id: this.detail.length,
            name: actSkill.trim(),
          });
        }
        this.skill = "";
      }
    },
    removeSkill(index) {
      this.detail.skills.splice(index, 1);
    },
    submitForm() {
      this.formsubmitted = true;
      refresh();
    },
    refresh() {
      alert("Jelenetkezésést továbbítottuk");
    },
  },
  computed: {
    valid() {
      this.isFormValid =
        !this.nameError &&
        this.detail.name !== "" &&
        this.detail.name !== undefined &&
        !this.emailError &&
        this.detail.email !== "" &&
        this.detail.email !== undefined &&
        !this.phoneError &&
        this.detail.phone !== "" &&
        this.detail.phone !== undefined &&
        !this.zipError &&
        this.detail.zip !== "" &&
        this.detail.zip !== undefined &&
        !this.cityError &&
        this.detail.city !== "" &&
        this.detail.city !== undefined &&
        !this.houseNumberError &&
        this.detail.houseNumber !== "" &&
        this.detail.houseNumber !== undefined &&
        this.detail.selectedJob !== "" &&
        this.detail.selectedJob !== undefined &&
        this.detail.selectedJob !== DEFAULT_SELECTION;

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

.skill {
  display: inline-block;
  margin: 1px 5px 0 0;
  padding: 3px 6px;
  border-radius: 20px;
  color: #777;
  background: cyan;
  cursor: pointer;
}
</style>

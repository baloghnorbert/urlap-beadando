<template>
  <p>Űrlap</p>
  <form @submit.prevent="">
    <div>
      <label>Név:</label>
      <input type="text" v-model.trim="name" @change="checkName" />
      <p class="error" v-if="in_valid_name">A név nem megfelelő</p>
    </div>

    <div>
      <label>Email:</label>
      <input
        type="email"
        @mouseenter="pwdCheck = true"
        @change="checkEmail"
        required
        v-model.trim="email"
      />
      <p class="error" v-if="emailError">
        Min 10 karakter hosszú legyen és a betűkön kívül számot is tartalmazzon!
      </p>
    </div>

    <div>
      <div>
        <label>Irányítószám:</label>
        <input
          type="number"
          @mouseenter="pwdCheck = true"
          @change="checkEmail"
          required
          v-model.trim="email"
        />
        <p class="error" v-if="emailError">
          Min 10 karakter hosszú legyen és a betűkön kívül számot is
          tartalmazzon!
        </p>
      </div>
      <div>
        <label>Város:</label>
        <input
          type=""
          @mouseenter="pwdCheck = true"
          @change="checkEmail"
          required
          v-model.trim="email"
        />
        <p class="error" v-if="emailError">
          Min 10 karakter hosszú legyen és a betűkön kívül számot is
          tartalmazzon!
        </p>
      </div>
    </div>

    <div>
      Igaz: <input type="radio" name="igazhamis" value="igaz" v-model="ih" />
      <br />
      Hamis: <input type="radio" name="igazhamis" value="hamis" v-model="ih" />
    </div>
    <p>Amit választottál: {{ ih }}</p>

    <div>
      Olvasás:
      <input type="checkbox" v-model="hobbies" value="olvasás" /> Sport:
      <input type="checkbox" v-model="hobbies" value="sport" /> Film:
      <input type="checkbox" v-model="hobbies" value="film" /> Utazás:
      <input type="checkbox" v-model="hobbies" value="utazás" />
    </div>
    <p>{{ hobbies }}</p>
    <p class="error" v-if="hobbies.length < 2">
      A fentiek közül legalább 2-t ki kell választani.
    </p>

    <div>
      <input type="date" v-model="datum" />
      <p>{{ datum }}</p>
    </div>

    <div>
      <select v-model="fejleszto">
        <option v-for="allas in allasok" :key="allas.nev" :value="allas.nev">
          {{ allas.nev }}
        </option>
      </select>
      <p class="error" v-if="fejleszto === ''">Még választottál!</p>
    </div>

    <input type="submit" value="Jelentkezés" v-if="kuldheto" />
  </form>
  <p>{{ name }}</p>
</template>

<script>
// @ is an alias to /src
import Form from "@/components/Form.vue";

const ONLY_LETTERS_REGEX = /^[a-zA-Z]+$/;
const EMAIL_ADDRESS_REGEX = /^[a-z0-9]{3,}@\S+\.[a-z0-9]{2,}$/;

export default {
  name: "Home",

  components: {
    Form,
  },
  data() {
    return {
      allasok: [{ nev: "Backend" }, { nev: "Frontend" }, { nev: "Full stack" }],

      fejleszto: "",
      datum: "",
      name: "",
      kuldheto: false,
      email: "",
      emailError: false,
      pwdError: false,
      ih: "",
      in_valid_name: false,
      hobbies: [],
    };
  },
  methods: {
    checkName() {
      let names = this.name.trim().split(" ");
      console.log("names:" + names);
      if (names.length < 2) {
        //NEM OK
        this.in_valid_name = true;
        return;
      }
      // ok
      this.in_valid_name =
        !ONLY_LETTERS_REGEX.test(names[0]) ||
        !ONLY_LETTERS_REGEX.test(names[1]) ||
        names[0].length < 2 ||
        names[1].length < 2;

      console.log("valid:" + this.in_valid_name);
      this.kuldheto = this.in_valid_name;
    },
    checkEmail() {
      this.emailError = !EMAIL_ADDRESS_REGEX.test(this.email);
    },
    pwdCheck() {
      let p = this.email;
      if (p.length > 9 && /[0-9]/.test(p) && /[a-zA-Z]/.test(p)) {
        this.pwdError = false;
      } else {
        this.pwdError = true;
      }
    },
  },
};
</script>
<style scoped>
.error {
  color: red;
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
</style>

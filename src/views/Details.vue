<template>
  <div id="data_container" class="col-75">
    <table id="data">
      <tr>
        <th class="bold" colspan="3" style="">Beküldött adatok</th>
      </tr>
      <tr>
        <td class="bold" rowspan="3">Személyes adatok:</td>
        <td>Név:</td>
        <td>{{ data.name }}</td>
      </tr>
      <tr>
        <td>E-mail:</td>
        <td>{{ data.email }}</td>
      </tr>
      <tr>
        <td>Telefonszám</td>
        <td>{{ data.phone }}</td>
      </tr>
      <tr>
        <td class="bold" rowspan="3">Cím adatok:</td>
        <td>Írányítószám:</td>
        <td>{{ data.zip }}</td>
      </tr>
      <tr>
        <td>Város:</td>
        <td>{{ data.city }}</td>
      </tr>
      <tr>
        <td>Utca házszám:</td>
        <td>{{ data.street + "" + data.houseNumber }}</td>
      </tr>
      <tr>
        <td class="bold">Választott pozíció:</td>
        <td colspan="2">{{ data.selectedJob }}</td>
      </tr>
      <tr>
        <td class="bold">Eddigi tapasztalatok:</td>
        <td id="experience" colspan="2">
          {{ data.experience }}
        </td>
      </tr>
      <tr>
        <td class="bold" rowspan="3">Technológiák:</td>
        <td colspan="2">
          <div
            class="skill"
            v-for="(skill, index) in data.skills"
            :key="index"
            :value="skill.name"
          >
            {{ skill.name }}
          </div>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "Details",
  props: {
    param: Object,
  },
  data() {
    return {
      data: this.param,
    };
  },
};
</script>

<style scoped>
.skill {
  display: inline-block;
  margin: 1px 5px 0 0;
  padding: 3px 6px;
  border-radius: 20px;
  color: rgba(119, 119, 119, 0.767);
  background: #eee;
}

#data_container {
  padding: 15px;
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
}
#data {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  text-align: center;
  /* width: 100%; */
}

#data td,
#data th {
  height: 50px;
  vertical-align: center;
  text-align: center;
  border: 1px solid rgb(190, 190, 190);
  padding: 10px 20px;
  word-wrap: break-word; /* All browsers since IE 5.5+ */
  overflow-wrap: break-word; /* Renamed property in CSS3 draft spec */
}

#data tr:nth-child(even) {
  background-color: #f2f2f2;
}

#data th {
  padding-top: 12px;
  padding-bottom: 12px;
  font-size: 20px;
  text-align: left;
  background-color: #04aa6d;
  color: white;
  text-align: center;
  font-weight: bold;
  text-transform: uppercase;
}

table {
  table-layout: fixed;
  border: 2px solid rgb(200, 200, 200);
  letter-spacing: 1px;
  font-size: 0.8rem;
  width: 75%;
}

caption {
  padding: 10px;
}

#data td.bold {
  font-weight: bold;
  text-transform: uppercase;
}

#data td#experience {
  word-wrap: break-word;
}
</style>
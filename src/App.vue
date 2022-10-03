<template>
  <div id="app">
    <h2 class="m-1">Конвертер перекладу систем числення онлайн</h2>
    <div class="d-grid gap-3">
      <b-form-input
        v-model="value"
        placeholder="Вихідні дані (десяткова система)"
        :type="'number'"
      ></b-form-input>
      <b-form-select
        placeholder="Підстава результату"
        v-model="system"
        :options="[
          {
            value: 2,
            text: 'Двійкова',
          },
          {
            value: 8,
            text: 'Вісімкова',
          },
        ]"
      >
      </b-form-select>
      <BButton @click="handle" class="w-100" variant="primary"
        >Показать</BButton
      >
    </div>
    <div class="border-top mt-2">
      <b-alert v-if="result" show>
        {{ value }} (10) = {{ result }} ({{ system }})
      </b-alert>

      <div v-if="result">
        <h1 v-if="result">Рішення</h1>
        <div v-for="(d, i) in decide" :key="i" class="m-2">
          <table>
            <tr>
              <td>{{ d.starter }}</td>
              <td>{{ system }}</td>
            </tr>
            <tr>
              <td>{{ d.deleter }}</td>
              <td>{{ d.result }}</td>
            </tr>
            <tr>
              <td>{{ d.remainder }}</td>
            </tr>
          </table>
        </div>
      </div>
      <div>
        <div>
          <b-link href="https://t.me/jonya5">Хочу покращити аплікацію</b-link>
        </div>
        <div>
          <b-link href="https://t.me/jonya5">GITHUB</b-link>
        </div>
        <div>Автори:</div>
        <div>
          <b-link href="https://www.instagram.com/yevhenii_kyrmyzy/"
            >jonya5</b-link
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/*eslint-disable */
import { ref } from 'vue';
export default {
  name: 'App',
  setup() {
    const value = ref(null);
    const system = ref(2);

    const result = ref(null);
    const decide = ref([]);

    const handle = () => {
      result.value = [];
      decide.value = [];
      let counter = value.value;
      let del = system.value;

      while (counter >= del) {
        decide.value.push({
          starter: counter,
          result: Math.floor(counter / del),
          deleter: Math.floor(counter / del) * del,
          remainder: counter - Math.floor(counter / del) * del,
        });

        counter = Math.floor(counter / del);
      }

      decide.value.push({
        remainder: counter,
      });

      decide.value.reverse().forEach((e) => {
        result.value.push(e.remainder);
      });

      decide.value.reverse();

      result.value = result.value.join('');
    };

    return {
      value,
      system,
      handle,
      decide,
      result,
    };
  },
};
</script>

<style>
#app {
  padding: 10px;
}

td {
  border: 1px solid black;
}
</style>

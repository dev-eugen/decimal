<template>
  <div id="app">
    <h2 class="m-1" style="margin-bottom: 20px">
      Конвертер перекладу систем числення онлайн
    </h2>
    <div class="d-grid gap-3">
      <h6 style="margin-bottom: -13px">Число:</h6>
      <b-form-input
        v-model="value"
        placeholder="Вихідні дані (десяткова система)"
        :type="'number'"
      ></b-form-input>
      <h6 style="margin-bottom: -13px">Початковий система:</h6>
      <b-form-select
        placeholder="Підстава результату"
        v-model="source_system"
        :options="[
          {
            value: false,
            text: 'Десятична',
          },
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

      <h6 style="margin-bottom: -13px">Очікувана система:</h6>
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
      <BButton
        :disabled="!value"
        @click="handle"
        class="w-100"
        variant="primary"
        >Показать</BButton
      >
    </div>
    <div class="border-top mt-2">
      <div v-if="result">
        <h1 v-if="result">Рішення</h1>
        <b-alert show v-if="convert_deciding.length">
          {{ value }} ({{ source_system }}) =
          <span v-for="(c, i) in convert_deciding" :key="i">
            <span v-if="i > 0"> + </span>
            {{ c.val }} * {{ c.system }}^{{ c.pow }}
          </span>
          = {{ value_converted }}
        </b-alert>
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

      <b-alert v-if="result" show>
        <h6>Відповідь:</h6>
        {{ value_converted }} (10) = {{ result }} ({{ system }})
      </b-alert>

      <div>
        <div>
          <b-link href="https://t.me/jonya5">Хочу покращити аплікацію</b-link>
        </div>
        <div>
          <b-link href="https://github.com/dev-eugen/decimal">GITHUB</b-link>
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
    const value_converted = ref(null);
    const system = ref(2);
    const source_system = ref(false);

    const result = ref(null);
    const decide = ref([]);
    const convert_deciding = ref([]);

    const handle = () => {
      convert_deciding.value = [];
      let ready_dd = 0;

      if (source_system.value) {
        let val = new String(value.value).split('');

        let i = val.length - 1;
        val.forEach((e) => {
          convert_deciding.value.push({
            val: e,
            system: source_system.value,
            pow: i,
          });

          ready_dd += new Number(
            new Number(e) * Math.pow(source_system.value, i)
          );

          i--;
        });

        value_converted.value = ready_dd;
      }

      console.log(convert_deciding.value);

      // convert.value =

      result.value = [];
      decide.value = [];
      let counter = source_system.value ? ready_dd : value.value;
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
      source_system,
      convert_deciding,
      value_converted,
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

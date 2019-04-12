<template>
  <div class="dito">
    <div class="store" v-for="(item, index) in events" :key="index">
      <div class="store-header">
        <p>{{moment(item.timestamp).format('DD/MM/YYYY') }}</p>
        <p>{{moment(item.timestamp).format('hh:mm') }}</p>
      </div>
      <div class="store-body">
        <div class="body-item" v-for="(custom, index) in item.custom_data" :key="index">
          <p>{{ custom.key }} {{ custom.value }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import _ from "lodash";
import moment from "moment";

export default {
  name: "Dito",
  data: () => ({
    events: [],
    moment: {}
  }),
  mounted() {
    this.products();
  },
  methods: {
    async products() {
      try {
        const res = await axios.get(
          `https://cors-anywhere.herokuapp.com/https://storage.googleapis.com/dito-questions/events.json`
        );
        const { events } = res.data;

        const newList = [];
        events.forEach(item => {
          const { custom_data } = item;
          const transaction = custom_data.find(c => c.key === "transaction_id");
          const details = newList.find(i => i.id === transaction.value);
          if (!details) {
            newList.push({
              ...item,
              id: transaction.value
            });
            return;
          }

          item.custom_data.map(value => {
            if (value.key !== "transaction_id") {
              details.custom_data.push(value);
            }
          });
        });

        this.events = _.orderBy(newList, ["timestamp"], ["desc"]);
        console.log(this.events);
        this.moment = moment;
      } catch (error) {
        console.log(`ERROR ${JSON.stringify(error)}`);
      }
    },

    getValueFromKey(key, customData) {
      return customData.find(custom => custom[key]);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.dito {
  display: flex;
  flex-direction: column;
}
.dito .store {
  margin: 2rem;
  background-color: #fff;
  padding: 2rem;
  border-radius: 5px;
}
.dito .store .store-header {
  display: flex;
  justify-content: center;
  align-items: center;
  border-bottom: 2px solid #DCDCDC;
}
.dito .store .store-header p {
  margin: 1rem;
}
.dito .store .store-body .body-item {
  border-bottom: 2px solid #DCDCDC;
}

</style>
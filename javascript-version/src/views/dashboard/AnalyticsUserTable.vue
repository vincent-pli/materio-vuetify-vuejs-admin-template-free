<script>
import axios from 'axios';

export default {
  data() {
    return {
      items: [],
      headers: [
        'NAME',
        'TASK',
        'MODEL',
        'STATUS',
        'ADDRESS',
        'AGE',
      ]
    }
  },

  methods: {
    getData() {
      axios.get('http://127.0.0.1:8000/list')
        .then(response => {
          // handle success
          console.log(response.data.res);
          for (const item of response.data.res){
            console.log(this.items)
            console.log(item)
            this.items.push({name: item.name, id: item.id, address: "http://127.0.0.1:" + item.port["8080/tcp"][0].HostPort, status: item.status, age: "120"})
          }
        })
        .catch(function (error) {
          // handle error
          console.log(error);
        })
        .finally(function () {
          // always executed
        });
    },
  },

  beforeMount() {
    // component is now mounted.
    this.getData()
  }
}


</script>

<template>
  <VCard>
    <VTable :headers="headers" :items="items" item-key="id" class="table-rounded" hide-default-footer
      disable-sort>
      <thead>
        <tr>
          <th v-for="header in headers" :key="header">
            {{ header }}
          </th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="row in items" :key="row.id">
          <!-- name -->

          <td>
            <div class="d-flex flex-column">
              <h6 class="text-sm font-weight-medium">
                {{ row.name }}
              </h6>
              <!-- <span class="text-xs">{{ row.post }}</span> -->
            </div>
          </td>

          <td class="text-sm" v-text="row.name" />
          <td class="text-sm" v-text="row.name" />
          <td class="text-sm" v-text="row.status" />
          <td class="text-sm">
            <a :href="row.address">
               address
            </a>
          </td>
          <td class="text-sm" v-text="row.age" />
          <!-- status -->
          <!-- <td>
            <VChip size="small" :color="statusColor[status[row.status]]" class="text-capitalize">
              {{ status[row.status] }}
            </VChip>
          </td> -->
        </tr>
      </tbody>
    </VTable>
  </VCard>
</template>

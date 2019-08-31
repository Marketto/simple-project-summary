<template>
    <b-table striped hover small :items="src" :fields="fields">
        <template slot="[name]" slot-scope="data">
            {{data.value}}
        </template>
        <template slot="[]" slot-scope="data">
            <img alt="missing badge" :src="data.value"/>
        </template>
    </b-table>
</template>
<script>

export default {
  name: 'websites',
  props: ['src'],
  data() {
    const noneBadge = 'https://img.shields.io/badge/none-lightgrey';
    const webFormatter = (uri, key) => uri ? `https://img.shields.io/website/${uri.replace('://', '/')}` : noneBadge;
    return {
      fields: [
        {
          key: 'name',
          label: 'Website name',
          sortable: true
        },
        {
          key: 'uri',
          label: 'Status',
          formatter: webFormatter 
        }
      ]
    };
  }
}
</script>
<template>
    <b-table striped hover small :items="src" :fields="fields" responsive="sm">
        <template slot="[name]" slot-scope="data">
            {{data.value}}
        </template>
        <template slot="[]" slot-scope="data">
            <a v-if="data.value.link && data.value.img" :href="data.value.link">
              <img alt="missing badge" :src="data.value.img"/>
            </a>
            <img alt="missing badge" :src="data.value.img" v-else-if="data.value.img"/>
            <img alt="none badge" src="https://img.shields.io/badge/none-lightgrey" v-else /> 
        </template>
    </b-table>
</template>
<script>

export default {
  name: 'websites',
  props: ['src'],
  data() {
    const webFormatter = uri => uri && {link: uri, img: `https://img.shields.io/website/${uri.replace('://', '/')}`};
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
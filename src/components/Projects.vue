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
  name: 'projects',
  props: ['src'],
  data() {
    const npmFormatter = (v, key, { npmId }) => npmId && {link: `https://www.npmjs.com/package/${npmId}`, img: `https://img.shields.io/npm/${key}/${npmId}.svg`};
    const chromeFormatter = (v, key, { chromeId }) => chromeId && { img: `https://img.shields.io/chrome-web-store/${key}/${chromeId}.svg`};
    const githubFormatter = (v, key, { githubId }) => githubId && { link: `https://github.com/${githubId}`, img: `https://img.shields.io/github/${key}/${githubId}.svg`};
    const davidFormatter = (v, key, {davidId}) => davidId && { link: `https://david-dm.org/${davidId}`, img: `https://david-dm.org/${davidId}/${key}.svg`};
    const sonarFormatter = (v, key, {sonarId}) => sonarId && { link: `https://sonarcloud.io/dashboard?id=${sonarId}`, img: `https://sonarcloud.io/api/project_badges/measure?project=${sonarId}&metric=${key}`};
    return {
      fields: [
        {
          key: 'name',
          label: 'Library name',
          sortable: true
        },
        {
          label: 'Latest version',
          key: 'v',
          formatter: (v, key, element) => {
            return npmFormatter(v, key, element) ||
              chromeFormatter(v, key, element) ||
              githubFormatter(v, `package-json/${key}`, element);
          }
        },
        {
          label: 'License',
          key: 'l',
          formatter: (v, key, element) => npmFormatter(v, key, element) || githubFormatter(v, 'license', element)
        },
        {
          label: 'Weekly downloads',
          link: (v, key, { npmId }) => npmId && `https://www.npmjs.com/package/${npmId}`,
          key: 'dw',
          formatter: npmFormatter 
        },
        {
          label: 'Monthly downloads',
          link: (v, key, { npmId }) => npmId && `https://www.npmjs.com/package/${npmId}`,
          key: 'dm',
          formatter: npmFormatter
        },
        {
          label: 'Total downloads',
          link: (v, key, { npmId }) => npmId && `https://www.npmjs.com/package/${npmId}`,
          key: 'dt',
          formatter: (v, key, element) => {
            return npmFormatter(v, key, element) ||
              chromeFormatter(v, 'users', element) ||
              githubFormatter(v, `downloads`, {githubId: `${element.githubId}/total`});
          }
        },
        {
          label: 'Sonar coverage',
          key: 'coverage',
          formatter: sonarFormatter
        },
        {
          label: 'Dependencies',
          key: 'status',
          formatter: davidFormatter
        },
        {
          label: 'Dev dependencies',
          key: 'dev-status',
          formatter: davidFormatter
        }
      ]
    };
  }
}
</script>
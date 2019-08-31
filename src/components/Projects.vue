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
  name: 'projects',
  props: ['src'],
  data() {
    const noneBadge = 'https://img.shields.io/badge/none-lightgrey';
    const npmFormatter = (v, key, { npmId }) => npmId ? `https://img.shields.io/npm/${key}/${npmId}.svg` : noneBadge;
    const chromeFormatter = (v, key, { chromeId }) => chromeId ? `https://img.shields.io/chrome-web-store/${key}/${chromeId}.svg` : noneBadge;
    const githubFormatter = (v, key, { githubId }) => githubId ? `https://img.shields.io/github/${key}/${githubId}.svg` : noneBadge;
    const davidFormatter = (v, key, {davidId}) => davidId ? `https://david-dm.org/${davidId}/${key}.svg` : noneBadge;
    const sonarFormatter = (v, key, {sonarId}) => sonarId ? `https://sonarcloud.io/api/project_badges/measure?project=${sonarId}&metric=${key}` : noneBadge;
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
                if (element.npmId) {
                    return npmFormatter(v, key, element);
                } else if (element.chromeId) {
                    return chromeFormatter(v, key, element);
                } else if (element.githubId) {
                    return githubFormatter(v, `package-json/${key}`, element);
                }
                return noneBadge;
          }
        },
        {
          label: 'License',
          key: 'l',
          formatter: (v, key, element) => element.npmId ? npmFormatter(v, key, element) : githubFormatter(v, 'license', element)
        },
        {
          label: 'Weekly downloads',
          key: 'dw',
          formatter: npmFormatter 
        },
        {
          label: 'Monthly downloads',
          key: 'dm',
          formatter: npmFormatter
        },
        {
          label: 'Total downloads',
          key: 'dt',
          formatter: (v, key, element) => {
                if (element.npmId) {
                    return npmFormatter(v, key, element);
                } else if (element.chromeId) {
                    return chromeFormatter(v, 'users', element);
                } else if (element.githubId) {
                    return githubFormatter(v, `downloads`, {githubId: `${element.githubId}/total`});
                }
                return noneBadge;
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
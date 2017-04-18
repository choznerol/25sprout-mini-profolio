<template lang="html">
  <div>

    <section v-for="i in Array(0,3,6)">
      <article v-for="P in Projects.slice(i,i+3)">
        <span v-if="P.Image">
          <a :href="P.ImageLink" target="_blank"><img :src="P.Image[0].thumbnails.large.url"></a>
        </span>
        <span v-else><img src="https://dl.airtable.com/UBrngFwQ6e89Ah9cnucY_large_github_155.png"></span>
        <h1>{{ P.Title }}</h1>
        <h2 v-html="P.Positions"></h2>
        <div v-html="P.Story"></div>
        <span class="skills" v-for="S in P.Skill"><code> {{ S }} </code></span>
      </article>
    </section>

  </div>
</template>

<script>
var _ = require('lodash');
var Airtable = require('airtable');
var LawrenceChou = new Airtable({apiKey: 'keyC0ndMttA3rW78D'}).base('appHQdjnnImr4xteP');

export default {

  data() {
    return {
      title: 'My Mini Profolio',
      Projects: [],
    }
  },

  created() {
    // Save data from Airtable upon created
    this.fetchData()

  },

  methods: {

    // Fetch data from Airtable
    fetchData(){
      let items = []
      LawrenceChou('Projects').select({
        maxRecords: 20,
        view: "25sprout"
      }).eachPage(function page(records, fetchNextPage) {
        items = items.concat(_.map(records, (record) => _.set(record.fields, 'id', record.id)));
        fetchNextPage();
      }, (err) => {
        if (err) { console.error(err); return; }
        this.Projects = items
      });
      console.log('Synced with Airtable')
      return
    },
  }
}
</script>

<!-- The design of this app was modified from Dudley Storey's example (http://codepen.io/dudleystorey/pen/umrnE) -->
<style src="../css/profolio.css"></style>
<style>
section {
  max-width: 1080px;
  margin: auto;
}

article {
  margin-bottom: 5%;
}

h1 {
  margin-bottom: 0px;
}

h2 {
  padding-bottom: 15px;
}

img {
  border: 1px grey solid;
}

/* CSS chip */
.skills {
  margin-right: 0.5em;
  color: grey;
  display: inline-block;
  padding: 0 12px;
  height: 24px;
  font-size: 16px;
  line-height: 24px;
  border-radius: 12px;
  background-color: #f1f1f1;
}

a {
  color: darkblue;
  border-bottom: 1px dotted #000;
  text-decoration: none;
}
</style>

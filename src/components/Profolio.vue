<template lang="html">
  <div>

    <h1>{{ title }}</h1>

    <section>

      <article class="breaking">
        <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/4273/earth-vs-the-flying-saucers.jpg" alt="Photograph of a flying saucer over the US Capitol building">
        <h1>Washington D.C. Attacked By Flying Saucers</h1>
        <h2>Dateline Washington D.C.</h2>
        <h3>Frank Bragg reporting</h3>
        <p>The country was brought to a standstill today when flying saucers – presumably from Mars, although Venusians have also been suspected – appeared over the nation’s capital, intent on destruction. Curiously, they only attacked Pennsylvania Avenue, and have not appeared elsewhere in the country.</p>
      </article>

      <article>
        <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/4273/bigfoot_1.jpg" alt="Photograph of a Bigfoot">
        <h1>Bigfoot Found, Shot, Killed</h1>
        <h2>Dateline Washington State</h2> <h3>Jessica Walsh reporting</h3>
        <p>The first conclusive proof of the elusive Sasquatch was found today, when one of the ape-men was found and killed by a hunter in the north-eastern corner of the state.</p>
        <p>The hunter plans to tour the pelt in the fall.</p>
      </article>

      <article>
        <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/4273/alligator-sewer_1.jpg" alt="Photograph of an alligator emerging from an open manhole cover">
        <h1>Nest of Alligators Found in New York Sewers</h1>
        <h2>Dateline New York City</h2>
        <h3>Ted Sturgis reporting</h3>
        <p>Years of rumours were confirmed yesterday when a nest of alligators were found in the sewers of New York City, just south of Times Square. The largest, which locals have dubbed “Mugsy”, measures over 21 feet long.</p>
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
      projects: [],
    }
  },
  created() {

    // Save data from Airtable
    this.syncAirtable()

  },
  methods: {

    // Fetch data from Airtable
    syncAirtable(){
      let items = []
      LawrenceChou('Projects').select({
        maxRecords: 20,
        view: "25sprout"
      }).eachPage(function page(records, fetchNextPage) {

        items = items.concat(_.map(records, (record) => _.set(record.fields, 'id', record.id)));

        fetchNextPage();
      }, (err) => {
        if (err) { console.error(err); return; }
        this.projects = items
      });
      console.log('Synced with Airtable')
      console.log(this.projects);
      return
    },

  }
}
</script>

<style lang="css">
/* Modified from Dudley Storey's example (http://codepen.io/dudleystorey/pen/umrnE) */
@import url(http://fonts.googleapis.com/css?family=Oswald:400,300);
@import url(http://fonts.googleapis.com/css?family=Merriweather);
@font-face {
  font-family: 'Queens Park';
  src: url('https://s3-us-west-2.amazonaws.com/s.cdpn.io/4273/queens-park.woff') format('woff');
  font-style: normal;
  font-weight: 400;
}
section { display: flex; }
body > h1 {
  font-family: Queens Park, serif;
  font-size: 4rem;
  text-align: center;
}
article {
  margin: 1rem;
  -ms-flex: 1 0 0px;
  flex: 1;
  /* shortcut for "flex-grow: 1". Note that it MUST be a unitless value to work; defined with a unit (e.g. flex: 1px) the shortcut would alter the flex-basis property. */
}
article.breaking {
  -ms-flex-grow: 2;
  flex: 2;
  /* shortcut for "flex-grow: 2" */
}

article img {
  width: 100%; height: auto;
}
article h1, article h2, article h3 { font-family: Oswald, serif; }
article h1 {
  font-size: 2rem;
  line-height: 1.3;
}
article h2, article h3 {
  font-size: 1.2rem;
  text-align: right;
  line-height: 1.3;
  margin: .3rem 0;
  font-weight: 300;
}
article h3 {
  font-size: 1.1rem;
}
article p {
  font-family: Merriweather;
  line-height: 1.6;
}
@media screen and (max-width: 750px) {
  body > h1 { font-size: 3rem; }
  section {
    -ms-flex-direction: column;
    -webkit-box-orient: vertical;
    flex-direction: column;
  }
}
</style>

<template>
  <div class="listing">
    <div class="container" id="app">
      <h3 class="text-center">{{msg}}</h3>
      <table class="table table-striped">
          <thead>
              <th>
                Term
              </th>
              <th>
                Translation
              </th>
          </thead>
          <tbody>
            <tr v-show="index > lowerLimit && index < upperLimit" v-for="(term, index) in terms">
              <td>{{ term.name }}</td>
              <td>
                <div class="form-group">
                  <input type="text" class="form-control" v-model="inputParams[index]" @blur="saveTranslation(index)">
                </div>
              </td>
            </tr>
          </tbody>
      </table>
      <button v-if="lowerLimit != 0" type="button" class="btn" @click="previous()">Previous</button>
      <button type="button" class="btn" @click="next()">Next</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Listing',
  data () {
    return {
      msg: 'Terms List',
      terms: [],
      translatedTerm: {
        testvalue: '',
      },
      inputParams: [],
      restRequest: false,
      lowerLimit: 0,
      upperLimit: 10
    }
  },
  computed: {
    totalPages: function() {
      return Math.ceil(this.resultCount / this.itemsPerPage)
    },
  },
  methods: {
      getPostsViaREST: function() {
        axios.get("https://jsonplaceholder.typicode.com/comments")
          .then(
            response => {this.terms = response.data}
          )
      },
      saveTranslation: function(index){
        var term = this.terms[index].ga;
        this.translatedTerm.filename = term.file;
        this.translatedTerm.testvalue = this.inputParams[index];
        this.translatedTerm.language = 'ga';
        this.translatedTerm.property = term.name;

        console.log(this.translatedTerm);
        axios.post('http://localhost:3000/api/language111',
          this.translatedTerm, // the data to post
          { headers: {
            'Content-type': 'application/x-www-form-urlencoded',
            }
          }).then(response => console.log("DONE"));
      },
      setPage: function(pageNumber) {
        this.currentPage = pageNumber
      },
      previous: function(){
        this.lowerLimit = this.lowerLimit - 10;
        this.upperLimit = this.upperLimit - 10;
      },
      next: function(){
        this.lowerLimit = this.lowerLimit + 10;
        this.upperLimit = this.upperLimit + 10;
      },
  },
  filters: {

  },
  mounted() {
    this.getPostsViaREST()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

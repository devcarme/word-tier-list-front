<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div class="main-content">
    <table class="word-list">
      <col class="word-column">
      <col class="definition-column">
      <col class="tier-column">
      <col class="actions-column">
      <thead>
        <tr>
          <th>Word</th>
          <th>Definition</th>
          <th>Tier</th>
          <th>Actions</th>
        </tr>
      <tr>
          <td><input type="text" v-model="name" placeholder="Name" /></td>
          <td class="word-definition"><input type="text" v-model="definition" placeholder="Definition" /></td>
          <td><input type="number" min="0" v-model="tier" placeholder="Tier" /></td>
          <button v-on:click="addWord">Add</button>
      </tr>
      <tr v-if="errorMessage !== ''">
        <td colspan="4">
          <p class="error-message">{{errorMessage}}</p>
        </td>
      </tr>
      </thead>
      <tbody v-bind:key="word._id" v-for="word in words">
        <tr class="word">
          <td class="word-name"> <input type="text"  v-model="word.name" :placeholder="word.name"   /></td>
          <td class="word-definition"><input type="text" v-model="word.definition" :placeholder="word.definition"/></td>
          <td class="word-tier"><input type="number" min="0" v-model="word.tier" :placeholder="word.tier"/></td>
          <button class="btn-remove" v-on:click="removeWord(word._id)">Remove</button>
          <button class="btn-edit" v-on:click="editWord(word)">Edit</button>
        </tr>
      </tbody>
    </table>
    </div>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Home',
  data() {
    return {
      msg: 'Word Tier List',
      errorMessage: '',
      words: [],
      name: '',
      definition: '',
      tier: '',
      disabledForm: true
    }
  },
  mounted() {
    this.getAllWords();
  },
  methods: {
    async getAllWords() {
        this.name = '';
        this.definition = '';
        this.tier = '';
        fetch(process.env.VUE_APP_API_BASE_URL + "words")
        .then(response => response.json())
        .then(data => {
          // eslint-disable-next-line vue/no-mutating-props
          this.words = data;
        })
        .catch(error => {
          if (error.message === 'Failed to fetch') {
            this.errorMessage = 'Le serveur est inaccessible';
          } else {
            this.errorMessage = error.message;
          }
        });
    },
    async removeWord(word_id) {
      fetch(process.env.VUE_APP_API_BASE_URL + "words/" + word_id, {
        method: 'DELETE',
      })
          .then(response => response.json())
          .then(this.getAllWords)
    },
    async addWord() {
      fetch(process.env.VUE_APP_API_BASE_URL + "words", {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json'
        },
        body: JSON.stringify({
          name: this.name,
          definition: this.definition,
          tier: this.tier
        })
      })
          .then(response => response.json())
          .then(this.getAllWords)
          .catch(error => {
            if (error.message === 'Failed to fetch') {
              this.errorMessage = 'Le serveur est inaccessible';
            } else {
              this.errorMessage = error.message;
            }
          });
    },
    async editWord(word) {
      fetch(process.env.VUE_APP_API_BASE_URL + "words", {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json'
        },
        body: JSON.stringify({word})
      })
          .then(response => response.json())
          .then(this.getAllWords)
          .catch(error => {
            if (error.message === 'Failed to fetch') {
              this.errorMessage = 'Le serveur est inaccessible';
            } else {
              this.errorMessage = error.message;
            }
          });
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
.main-content {
  display: flex;
  flex-direction: row;
  justify-content: center;
  text-align: start;
}
button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 5px;
  margin: 10px;
  cursor: pointer;
}
.btn-remove {
  background-color: #ff0000;
}
.word-list {
  width: 100%;
  border-collapse: collapse;
}
.word-list th, .word-list td {
  border: 1px solid #ddd;
  padding: 8px;
}
.word-list tr:nth-child(even){background-color: #f2f2f2;}
.word-list tr:hover {background-color: #ddd;}
.word-list th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #42b983;
  color: white;
}
input {
  width: 100%;
}
input[type="number"] {
  width: 85%;
  border: 1px solid #42b983;
  border-radius: 5px;
  padding: 5px;
}
input[type="text"] {
  width: 90%;
  border: 1px solid #42b983;
  border-radius: 5px;
  padding: 10px;
  height: 30px;
  font-size: 20px;
}
.error-message {
  color: red;
  font-size: 24px;
  text-align: center;
}
.tier-column {
  width: 60px;
}
.actions-column {
  width: 200px;
}
.word-column {
  width: 200px;
}
.word-definition input[type="text"] {
  width: 98%;
}
.btn-edit {
  background-color: orange;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 5px;
  margin: 10px;
  cursor: pointer;
}
/*Add hover effect on button*/
button:hover {
  opacity: 0.8;
}
</style>

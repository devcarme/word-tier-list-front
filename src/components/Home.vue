<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div class="main-content">
    <table class="word-list">
      <thead>
        <tr>
          <th>Word</th>
          <th>Definition</th>
          <th>Tier</th>
          <th>Actions</th>
        </tr>
      <tr>
          <td><input type="text" v-model="name" placeholder="Name" /></td>
          <td><input type="text" v-model="definition" placeholder="Definition" /></td>
          <td><input type="number" v-model="tier" placeholder="Tier" /></td>
          <button v-on:click="addWord">Add</button>
      </tr>
      </thead>
      <tbody v-bind:key="word._id" v-for="word in words">
        <tr class="word">
          <td class="word-name"> <input type="text"  v-model="nameEdit" :placeholder="word.name"   />{{ word.nameEdit }}</td>
          <td class="word-definition"><input type="text" v-model="definitionEdit" :placeholder="word.definition" disabled="disabledForm"/> {{ word.definitionEdit }}</td>
          <td class="word-tier"><input type="text" v-model="tierEdit" :placeholder="word.tier" disabled="disabledForm"/> {{ word.tierEdit }}</td>
          <button class="btn-remove" v-on:click="removeWord(word._id)">Remove</button>
          <button class="btn-edit" v-on:click="editWord(word._id)">Edit</button>
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
      words: [],
      name: '',
      definition: '',
      tier: '',
      nameEdit: '',
      definitionEdit: '',
      tierEdit: '',
      disabledForm: true,
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
        fetch('http://127.0.0.1:5000/words')
        .then(response => response.json())
        .then(data => {
          // eslint-disable-next-line vue/no-mutating-props
          this.words = data;
        })
    },
    async removeWord(word_id) {
      fetch('http://127.0.0.1:5000/words/' + word_id, {
        method: 'DELETE'
      })
          .then(response => response.json())
          .then(this.getAllWords)
    },
    async addWord() {
      fetch('http://127.0.0.1:5000/words', {
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
          .catch(error => console.error(error))
    },
    async editWord() {
      fetch('http://127.0.0.1:5000/words', {
        method: 'PUT',
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
          .catch(error => console.error(error))
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
.word-name {
  width: 20%;
}
.word-definition {
  width: 40%;
}
.word-tier {
  width: 20%;
}
input {
  width: 100%;
}
input[type="number"] {
  width: 50px;
  border: 1px solid #42b983;
  border-radius: 5px;
  padding: 5px;
}
/*Input styled for the word list*/
input[type="text"] {
  width: 85%;
  border: 1px solid #42b983;
  border-radius: 5px;
  padding: 10px;
  height: 30px;
}

</style>

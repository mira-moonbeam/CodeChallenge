<style src="./components/style.css"></style>
<template src="./components/template.html"></template>

<script >
export default {
  name: 'ContactsApp',

  data() {
    return {
      favorites: {},
      sortOrder: "ID",
      listData: [],
      hidden: {},
      search: ""
    }
  },
	
  mounted() {
    const fontAwesomeScript = document.createElement('script')
    fontAwesomeScript.setAttribute('src', 'https://kit.fontawesome.com/92ab964b0d.js')
    fontAwesomeScript.setAttribute('crossorigin', 'anonymous')
    document.head.appendChild(fontAwesomeScript)

    fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => response.json())
    .then(data => {this.listData = data})
  },
  
  computed: {
    orderedListOptions: function() {
      return {
        "ID": () => {return this.listData},
        "ID - Reverse": () => {return this.listData.slice().reverse()},
        "Alphabetical": () => {return this.listData.slice().sort(function(a, b) {return a.name.localeCompare(b.name)})},
        "Alphabetical - Reverse": () => {return this.listData.slice().sort(function(a, b) {return b.name.localeCompare(a.name)})},
      }
    },
    
    filteredList: function() {
      const search = this.search.toLowerCase()
      const list = this.sort(this.sortOrder)
      
      if (!search) {
        return list
      }
      
      return list.filter((item) => {
        return item.name.toLowerCase().includes(search)
      })
    },
    
    checkIfHidden() {
      return (id) => {
        if (!(id in this.hidden)) {
          return true
        } 
        return this.hidden[id]
      }
    },
		
    checkIfFav() {
      return (id) => {
        if (!(id in this.favorites)) {
          return true
        } 
        return this.favorites[id]
      }
    },
		
		favEmpty: function() {
			if (Object.keys(this.favorites).length == 0) {
				return true
			}
			
			for (var val of Object.values(this.favorites)) {
					if (!val) {		
						return false
					}
			}
			return true
		}
  },
  
  methods: {
    sort(sortOrder) {
      return this.orderedListOptions[sortOrder]()
    },
    
    showHide(id) {
      if(id in this.hidden) {
        this.hidden[id] = !this.hidden[id]
      } else {
        this.hidden[id] = false
      }
    },
    
    fav(id) {
      if(id in this.favorites) {
        this.favorites[id] = !this.favorites[id]
      } else {
        this.favorites[id] = false
      }
    },
  },
}
</script>
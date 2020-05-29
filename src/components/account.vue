

<template>
  <div class="container">
    <section>
	  <header class="header">
		<input class="new-contact" 
		  v-model="newContact"
		  v-on:keyup.enter="addContact"
		  placeholder="Добавить контакт..." autofocus>
	  </header>
    <section class="main">
	  <ul class="contact-list">
		  <li v-for="contact in filteredContacts"
			:class="{ completed: !contact.completed, editing: contact == editedContact}">
			  <div class="view">
                 <input class="toggle" type="checkbox" v-model="contact.completed" />
				 <label @dblclick="editContact(contact)">{{ contact.title }}</label>
				 <button class="destroy" @click="removeContact(contact)"></button>
			  </div>
				 <input class="edit" type="text"
					v-model="contact.title"
					@blur="doneEdit(contact)"
					@keyup.enter="doneEdit(contact)" />
		   </li>
	  </ul>
				
	</section>
      <footer class="footer">
	  </footer>
	</section>

  </div>
</template>

<script>
  const STORAGE_KEY = 'contact-storage';
  import { setTimeout } from 'timers';
  export default {
    created () {
      this.contacts = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
  },
    computed: {


        filteredContacts() {
          if(this.visibility === 'all') {
          return this.contacts;
        }
          else if(this.visibility === 'active') {
          return this.contacts.filter(function(contact) {
          return !contact.completed;
          });
        }
          else {
          //completed
          return this.contacts.filter(function(contact) {
          return contact.completed;
        })
      }
    }
  },
   data: () => {
    return {
      newContact: '',
      contacts:[],
      editedContact: null,
      visibility: 'all',      
    }
  },
   methods: {
     addContact() {
       this.contacts.splice(0, 0, 
	  {
		  title: this.newContact, 
		  completed: 'true', 
		  id: this.contacts.length }
	  );
       this.newContact = '';
       localStorage.setItem(STORAGE_KEY, JSON.stringify(this.contacts));
       this.$store.state.post2 = this.contacts;
      },
     removeContact(contact) {
       this.contacts.splice(this.contacts.indexOf(contact), 1);
       localStorage.setItem(STORAGE_KEY, JSON.stringify(this.contacts));
    },
     editContact(contact) {
       this.editedContact = contact;
    },
     doneEdit(contact) {
       if(!this.editedContact) {
       return
      }
       this.editedContact = null;
       contact.title = contact.title.trim();
       if (!contact.title) {
         this.removeContact(contact);
      }
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.contacts));
     }
    }
  }
</script>

<style lang="scss" scoped>

hr {
	margin: 20px 0;
	border: 0;
	border-top: 1px dashed #c5c5c5;
	border-bottom: 1px dashed #f7f7f7;
}



html,
body {
	margin: 0;
	padding: 0;
}



body {
	font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
	line-height: 1.4em;
	background: #f5f5f5;
	color: #4d4d4d;
	min-width: 230px;
	max-width: 550px;
	margin: 0 auto;
	-webkit-font-smoothing: antialiased;
	-moz-font-smoothing: antialiased;
	font-smoothing: antialiased;
	font-weight: 300;
}

button,
input[type="checkbox"] {
	outline: none;
}

.hidden {
	display: none;
}

.new-contact,
.edit {
	position: relative;
	margin: 0;
	width: 100%;
	font-size: 24px;
	font-family: inherit;
	font-weight: inherit;
	line-height: 1.4em;
	border: 0;
	outline: none;
	color: inherit;
	padding: 6px;
	border: 1px solid #999;
	box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
	box-sizing: border-box;
	-webkit-font-smoothing: antialiased;
	-moz-font-smoothing: antialiased;
	font-smoothing: antialiased;
}

.new-contact {
	padding: 16px 16px 16px 60px;
	border: none;
	background: rgba(16, 190, 103, 0.003);
	box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
}

.main {
	position: relative;
	z-index: 2;
	border-top: 1px solid #4a2c81;
}

label[for='toggle-all'] {
	display: none;
}

.toggle-all {
	position: absolute;
	top: -55px;
	left: -12px;
	width: 60px;
	height: 34px;
	text-align: center;
	border: none; /* Mobile Safari */
}

.toggle-all:before {
	content: '❯';
	font-size: 22px;
	color: #e6e6e6;
	padding: 10px 27px 10px 27px;
}

.toggle-all:checked:before {
	color: #737373;
}

.contact-list {
	margin: 0;
	padding: 0;
	list-style: none;
}

.contact-list li {
	position: relative;
	font-size: 18px;
	border-bottom: 1px solid #ededed;
}

.contact-list li:last-child {
	border-bottom: none;
}

.contact-list li.editing {
	border-bottom: none;
	padding: 0;
}

.contact-list li.editing .edit {
	display: block;
	width: 506px;
	padding: 13px 17px 12px 17px;
	margin: 0 0 0 43px;
}

.contact-list li.editing .view {
	display: none;
}

.contact-list li .toggle {
	text-align: center;
	width: 40px;
	/* auto, since non-WebKit browsers doesn't support input styling */
	height: auto;
	position: absolute;
	top: 0;
	bottom: 0;
	margin: auto 0;
	border: none; /* Mobile Safari */
	-webkit-appearance: none;
	appearance: none;
}

.contact-list li .toggle:after {
	content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="50" fill="none" stroke="#ededed" stroke-width="3"/></svg>');
}

.contact-list li .toggle:checked:after {
	content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="50" fill="none" stroke="#bddad5" stroke-width="3"/><path fill="#5dc2af" d="M72 25L42 71 27 56l-4 4 20 20 34-52z"/></svg>');
}

.contact-list li label {
	white-space: pre-line;
	word-break: break-all;
	padding: 15px 60px 15px 15px;
	margin-left: 45px;
	display: block;
	line-height: 1.2;
	transition: color 0.4s;
}

.contact-list li.completed label {
	color: #d9d9d9;
	text-decoration: line-through;
}

.contact-list li .destroy {
	display: none;
	position: absolute;
	top: 0;
	right: 10px;
	bottom: 0;
	width: 40px;
	height: 40px;
	margin: auto 0;
	font-size: 30px;
	color: #cc9a9a;
	margin-bottom: 11px;
	transition: color 0.2s ease-out;
}

.contact-list li .destroy:hover {
	color: #af5b5e;
}

.contact-list li .destroy:after {
	content: 	'\d7';
}

.contact-list li:hover .destroy {
	display: block;
}

.contact-list li .edit {
	display: none;
}

.contact-list li.editing:last-child {
	margin-bottom: -1px;
}

.footer {
	color: #777;
	padding: 10px 15px;
	height: 20px;
	text-align: center;
	border-top: 1px solid #e6e6e6;
}

.footer:before {
	content: '';
	position: absolute;
	right: 0;
	bottom: 0;
	left: 0;
	height: 50px;
	overflow: hidden;
	box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2),
	            0 8px 0 -3px #f6f6f6,
	            0 9px 1px -3px rgba(0, 0, 0, 0.2),
	            0 16px 0 -6px #f6f6f6,
	            0 17px 2px -6px rgba(0, 0, 0, 0.2);
}

.contact-count {
	float: left;
	text-align: left;
}

.contact-count strong {
	font-weight: 300;
}

.filters {
	margin: 0;
	padding: 0;
	list-style: none;
	position: absolute;
	right: 0;
	left: 0;
}

.filters li {
	display: inline;
}

.filters li a {
	color: inherit;
	margin: 3px;
	padding: 3px 7px;
	text-decoration: none;
	border: 1px solid transparent;
	border-radius: 3px;
}

.filters li a.selected,
.filters li a:hover {
	border-color: rgba(175, 47, 47, 0.1);
}

.filters li a.selected {
	border-color: rgba(175, 47, 47, 0.2);
}

.clear-completed,
html .clear-completed:active {
	float: right;
	position: relative;
	line-height: 20px;
	text-decoration: none;
	cursor: pointer;
	position: relative;
}

.clear-completed:hover {
	text-decoration: underline;
}

.info {
	margin: 65px auto 0;
	color: #bfbfbf;
	font-size: 10px;
	text-shadow: 0 1px 0 rgba(255, 255, 255, 0.5);
	text-align: center;
}

.info p {
	line-height: 1;
}

.info a {
	color: inherit;
	text-decoration: none;
	font-weight: 400;
}

.info a:hover {
	text-decoration: underline;
}



@media (max-width: 430px) {
	.footer {
		height: 50px;
	}

	.filters {
		bottom: 10px;
	}
}


</style>
  
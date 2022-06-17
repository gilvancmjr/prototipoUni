<template>
  <v-data-table
    :headers="headers"
    :items="users"
    sort-by="calories"
    class="elevation-1"
    :search="search"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Usuários</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-col cols="6" sm="6" md="4" lg="6">
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
            outlined
            dense
          ></v-text-field>
        </v-col>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              id="account"
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
              
            >
              <v-icon dark left> mdi-account-plus </v-icon>
              Adicionar usuário
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <h3>Nome</h3>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field
                      v-model = "editedItem.nome"
                      outlined
                      dense
                      placeholder="Informe seu nome"
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <h3>E-mail</h3>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field
                    v-model = "editedItem.email"
                      outlined
                      dense
                      placeholder="Informe seu e-mail"
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <h3>Senha</h3>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field
                      v-model="editedItem.password"
                      :append-icon="show2 ? 'mdi-eye' : 'mdi-eye-off'"
                      :type="show2 ? 'text' : 'password'"
                      name="input-10-1"
                      counter
                      outlined
                      dense
                      @click:append="show2 = !show2"
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="12" sm="12" md="12">
                    <h3>Confirme sua senha</h3>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field
                      v-model="password"
                      :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                      :type="show1 ? 'text' : 'password'"
                      name="input-10-1"
                      counter
                      outlined
                      dense
                      @click:append="show1 = !show1"
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="12" sm="6" md="6">
                    <h3>Permissão</h3>
                  </v-col>
                  <v-col cols="12" sm="6" md="6">
                    <h3>Status</h3>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="12" sm="6" md="6">
                    <v-autocomplete
                      outlined
                      v-model="editedItem.permissao"
                      dense
                      :items="itemsPermissao"
                      placeholder="Selecione..."
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="6" md="6">
                    <v-autocomplete
                      outlined
                      v-model="editedItem.status"
                      :items="itemsStatus"
                      dense
                      placeholder="Selecione..."
                    ></v-autocomplete>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-col cols="12" sm="6" md="3">
                <v-btn color="primary" dark class="mb-2" @click="close">
                  <v-icon> mdi-arrow-left </v-icon> Voltar
                </v-btn>
              </v-col>

              <v-col cols="12" sm="6" md="9">
                <v-btn color="primary" dark class="mb-2" @click="save">
                  <v-icon> mdi-account-check </v-icon>
                  Salvar
                </v-btn>
              </v-col>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5"
              >Deseja realmente excluir o usúario?</v-card-title
            >
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="primary"
                dark
                class="mb-2"
                @click="deleteItemConfirm"
              >
                <v-icon> mdi-trash-can-outline </v-icon> Sim, excluir
                usuário</v-btn
              >
              <v-btn color="primary" dark class="mb-2" @click="closeDelete"
                >Cancelar</v-btn
              >

              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
      <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
    </template>
  </v-data-table>
</template>
<script>
import Axios from "axios";
export default  {
  data: () => ({
    search: "",
    dialog: false,
    dialogDelete: false,
    show1: false,
    show2: false,
    password: "",
    itemsStatus:[ 'Ativo', 'Inativo'],
    itemsPermissao:['Administrador', 'Usuário'],
    headers: [
      {
        text: "Nome",
        align: "start",
        sortable: false,
        value: "nome",
      },
      { text: "E-mail", value: "email" },
      { text: "Permissão", value: "permissao" },
      { text: "Status", value: "status" },
      { text: "Actions", value: "actions", sortable: false },
    ],
    users: [
      {
        nome: "Gilvan",
        email: "Teste@gmail.com",
        permissao: "Administrador",
        status: "ativo",
      },
    ],
    editedIndex: -1,
    editedItem: {
      nome: "",
      email: "",
      permissao: "",
      status: "",
      password:"",

    },
    defaultItem: {
      nome: "",
      email: "",
      permissao: "",
      status: "",
      password:"",
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1
        ? "Cadastrar usuário"
        : "Atualizar  usuário";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      Axios.get('http://localhost:3000/cadastro').then((response) => {
        console.log(response.data)
        this.users = response.data
        
      });
    },

    editItem(item) {
      this.editedIndex = this.users.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.users.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.users.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if(this.editedItem.password == this.password){
        Axios.post('http://localhost:3000/cadastro',this.editedItem).then((response) => {
        console.log(response.data)
        this.users = response.data
        this.initialize()
      });
      }
      console.log(this.editedItem)
    
      this.close();
    },
    
  },
};
</script>

<style>
#account {
  margin-top: 5px;
}
</style>
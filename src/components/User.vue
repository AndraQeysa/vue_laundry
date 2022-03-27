<template>
    <div>
        <main>
            <div class="container-fluid px-4">
                <h1 class="mt-4">Data User</h1>
                <div class="card mb-4">
                    <div class="card-body">
                        <a class="btn btn-success" v-b-modal.modal_user @click="Add()">Tambah User</a>
                        <table class="table">
                            <tr>
                                <td>ID User</td>
                                <td>Nama</td>
                                <td>Username</td>
                                <td>Role</td>
                                <td>Outlet</td>
                                <td>Aksi</td>
                            </tr>
                            <tr v-for="ser in user" :key="ser">
                                <td>{{ ser.id_user }}</td>
                                <td>{{ ser.nama }}</td>
                                <td>{{ ser.username }}</td>
                                <td>{{ ser.role }}</td>
                                <td>{{ ser.id_outlet }}</td>
                                <td>
                                    <a v-b-modal.modal_user href="#" class="btn btn-info" @click="Edit(ser)">Ubah</a>
                                    <a href="#" class="btn btn-danger" @click="Delete(ser.id)">Hapus</a>
                                </td>
                            </tr>
                        </table>
                    </div>
                </div>
            </div>
        </main>

        <b-modal 
            id="modal_user" 
            ref="modal" 
            title="Form User" 
            size="md" 
            @ok="Save">
            <form>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="nama" placeholder="Masukkan Nama User" id="inputNama" class="form-control" type="text"/>
                    <label for="inputNama">Nama</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="username" placeholder="Masukkan Username" id="inputUser" class="form-control" type="text"/>
                    <label for="inputUser">Username</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="password" placeholder="Masukkan Password" id="inputPass" class="form-control" type="password"/>
                    <label for="inputPass">Password</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <select v-model="role" class="form-control">
                        <option selected value="">-- Pilih Role --</option>
                        <option value="owner">Owner</option>
                        <option value="admin">Admin</option>
                        <option value="kasir">Kasir</option>
                    </select>

                    <label for="inputRole">Role</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <b-form-select v-model="id_outlet" :options="data_outlet" class="form-control"></b-form-select>
                    <label for="inputOutlet">Outlet</label>
                </div>
            </form>
        </b-modal>
    </div>
</template>
<script>
module.exports =  {
    data: function(){
        return {
            id_outlet: "",
            id_user: "",
            nama: "",
            username:"",
            password: "",
            role:"",
            data_outlet: [],
            user: [],
            action:"",
            fields: ["id", "nama", "username", "role", "outlet", "action"],
        }
    },
    methods: {
        getData: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

          axios.get(base_url + '/user', config)
          .then( response => {
              console.log(response);
            if(response.data.success == true){
                this.user = response.data.data.user;
            }
          })

        },
        outletDropdown: function() {
            let config = {
                headers: {
                    Authorization: "Bearer " + this.$cookies.get("Authorization"),
                },
            }
            axios.get(base_url + '/outlet', config)
            .then((response) => {
                let json_outlet = response.data.data.outlet;
                let list_outlet = [
                    {
                        value: "",
                        text: "--Pilih Outlet--",
                    },
                ];
                json_outlet.forEach((element) => {
                    list_outlet.push({
                        value: element.id_outlet,
                        text: element.nama_outlet
                    });
                });
                this.data_outlet = list_outlet;
            })
        },
        Add: function(){
            this.action = "insert";
            this.id_outlet = "";
            this.id_user = "";
            this.nama = "";
            this.username = "";
            this.password = "";
            this.role = "";
        },
        Edit: function(item){
            this.action = "update";
            this.id_outlet = item.id_outlet;
            this.id_user = item.id;
            this.nama = item.nama;
            this.username = item.username;
            this.role = item.role;
        },
        Save: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

            let form = {
                "id_outlet": this.id_outlet,
                "nama": this.nama,
                "username": this.username,
                "password": this.password,
                "role": this.role,
            }

            //logika method post/get (insert /update)
            if(this.action == "insert"){
                axios.post(base_url + '/user', form, config)
                .then( response => {
                    this.getData();
                    alert(response.data.message);
                })
                .catch((error) => {
                    console.log(error);
                })
            } else { //update
                axios.put(base_url + '/user/' + this.id, form, config)
                .then( response => {
                    this.getData();
                    alert(response.data.message);
                })
                .catch((error) => {
                    console.log(error);
                })
            }
            
        },
        Delete: function(id){
           if(confirm("Apakah anda yakin menghapus data user ini?")){

                let config = {
                    headers : {
                    "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                    }
                }

                axios.delete(base_url + '/user/' + id, config)
                .then( response => {
                    alert(response.data.message);
                })

                this.getData();
           }
        }

    },
    mounted() {
        this.getData();
        this.outletDropdown();
    },
}
</script>
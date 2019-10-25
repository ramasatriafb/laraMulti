<template>
    <div class="container">
        <div class="row mt-5">
          <div class="col-md-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Users Table</h3>

                <div class="card-tools">
                    <button class="btn btn-success" >Add New <i class="fas fa-user-plus fa-fw"></i></button>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                  <tbody>
                    <tr>
                        <th>ID</th>
                        <th>Name</th>
                        <th>Email</th>
                        <th>Type</th>
                        <th>Registered At</th>
                        <th>Modify</th>
                  </tr>


                  <tr v-for="user in users.data" :key="user.id">

                    <td>{{user.id}}</td>
                    <td>{{user.name}}</td>
                    <td>{{user.email}}</td>
                    <td>{{user.type}}</td>
                    <td>{{user.created_at }}</td>

                    <td>
                        
                    </td>
                  </tr>
                </tbody></table>
              </div>
              <!-- /.card-body -->
              <div class="card-footer">
                  <!-- <pagination :data="users" @pagination-change-page="getResults"></pagination> -->
              </div>
            </div>
            <!-- /.card -->
          </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                users : {},
                // form: new Form({
                //     id:'',
                //     name : '',
                //     email: '',
                //     password: '', 
                //     type: '',
                //     bio: '',
                //     photo: ''
                // })
            }
        },
        methods: {
            loadUsers(){
                    axios.get("api/user").then(({ data }) => (this.users = data));
            },

            // createUser(){
            //     this.form.post('api/user');
            // }
        },
        created() {
           this.loadUsers();
        }

    }
</script>

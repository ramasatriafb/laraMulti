<style>
.widget-user-header{
    background-position: center center;
    background-size: cover;
    height: 250px !important;
}
.widget-user .card-footer{
    padding: 0;
}

</style>

<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12 mt-3">
                <div class="card card-widget widget-user">
                    <!-- Add the bg color to the header using any of the bg-* classes -->
                    <div class="widget-user-header text-white" style="background-image:url('./img/photo1.png')">
                        <h3 class="widget-user-username text-left">Elizabeth Pierce</h3>
                        <h5 class="widget-user-desc text-left  ">Web Designer</h5>
                    </div>
                    <div class="widget-user-image">
                        <img class="img-circle" src="" alt="User Avatar">
                    </div>
                    <div class="card-footer">
                        <div class="row">
                        <div class="col-sm-4 border-right">
                            <div class="description-block">
                            <h5 class="description-header">3,200</h5>
                            <span class="description-text">SALES</span>
                            </div>
                            <!-- /.description-block -->
                        </div>
                        <!-- /.col -->
                        <div class="col-sm-4 border-right">
                            <div class="description-block">
                            <h5 class="description-header">13,000</h5>
                            <span class="description-text">FOLLOWERS</span>
                            </div>
                            <!-- /.description-block -->
                        </div>
                        <!-- /.col -->
                        <div class="col-sm-4">
                            <div class="description-block">
                            <h5 class="description-header">35</h5>
                            <span class="description-text">PRODUCTS</span>
                            </div>
                            <!-- /.description-block -->
                        </div>
                        <!-- /.col -->
                        </div>
                        <!-- /.row -->
                    </div>
                </div>
                <div class="card">
                <div class="card-header p-2">
                    <ul class="nav nav-pills">
                    <li class="nav-item"><a class="nav-link" href="#activity" data-toggle="tab">Activity</a></li>
                    <li class="nav-item"><a class="nav-link active" href="#settings" data-toggle="tab">Settings</a></li>
                    </ul>
                </div><!-- /.card-header -->
                <div class="card-body">
                    <div class="tab-content">
                    <div class="tab-pane" id="activity">
                        <!-- Post -->
                        <div class="post">
                            <h2>Display User Activity</h2>
                        </div>
                        <!-- /.post -->
                    </div>
                    <!-- /.tab-pane -->

                    <div class="tab-pane active" id="settings">
                        <form class="form-horizontal">
                        <div class="form-group row">
                            <label for="inputName" class="col-sm-2 col-form-label">Name</label>
                            <div class="col-sm-10">
                            <input v-model="form.name" type="text" class="form-control" :class="{ 'is-invalid': form.errors.has('name') }" id="name" name="name" placeholder="Name">
                            <has-error :form="form" field="name"></has-error>
                            </div>
                        </div>
                        <div class="form-group row">
                            <label for="inputEmail" class="col-sm-2 col-form-label">Email</label>
                            <div class="col-sm-10"> 
                            <input v-model="form.email" type="email" class="form-control" :class="{ 'is-invalid': form.errors.has('email') }" id="email" name="email" placeholder="Email">
                            <has-error :form="form" field="email"></has-error>
                            </div>
                        </div>
                        <div class="form-group row">
                            <label for="Bio" class="col-sm-2 col-form-label">Email</label>
                            <div class="col-sm-10"> 
                            <textarea v-model="form.bio" name="bio" id="bio"
                            placeholder="Short bio for user (Optional)"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
                            <has-error :form="form" field="bio"></has-error>
                            </div>
                         </div>
                        <div class="form-group row">
                            <label for="inputPhoto" class="col-sm-2 col-form-label">Profile Photo</label>
                            <div class="col-sm-10">
                            <input type="file" @change="updateProfile" name="photo" class="form-control" id="photo">
                            </div>
                        </div>
                        <div class="form-group row">
                            <label for="inputPassword" class="col-sm-2 col-form-label">Passport (leave empty if not changing</label>
                            <div class="col-sm-10">
                            <input v-model="form.password" type="password" class="form-control" :class="{ 'is-invalid': form.errors.has('password') }" id="password" name="password" placeholder="Password">
                            <has-error :form="form" field="password"></has-error>
                            </div>
                        </div>
                        <div class="form-group row">
                            <div class="offset-sm-2 col-sm-12">
                            <button @click.prevent="updateInfo" type="submit" class="btn btn-success">Update</button>
                            </div>
                        </div>
                        </form>
                    </div>
                    <!-- /.tab-pane -->
                    </div>
                    <!-- /.tab-content -->
                </div><!-- /.card-body -->
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default { 
        data(){
            return {
                form: new Form({
                    id:'',
                    name : '',
                    email: '',  
                    password: '', 
                    type: '',
                    bio: '',
                    photo: ''
                })
            }
        },
        mounted() {
            console.log('Component mounted.')
        },

        methods:{
            updateInfo(){
                this.$Progress.start();
                this.form.put('api/profile')
                .then(()=>{
                    Swal.fire(
                        'Updated!',
                        'Information has been updated.',
                        'success'
                    )
                    this.$Progress.finish();
                })
                .catch(()=>{
                    this.$Progress.fail();
                })
            },
            updateProfile(e){
                let file = e.target.files[0];
                
                let reader = new FileReader();
                
                if(file['size'] < 2111775){
                    reader.onloadend = (file) => {
                        // console.log(reader.result);
                        this.form.photo = reader.result;
                        console.log(this.form.photo);
                    }
                    reader.readAsDataURL(file);
                }else{
                     Swal.fire({
                         type: 'error',
                         title: 'Oops..',
                         text: 'You are uploading a large file',
                     })
                }
            } 
        },

        created(){
            axios.get('api/profile')
            .then(({data})=> (this.form.fill(data)));
        }
    }
</script>

<style>
    .user-widget{
        background-position: center center;
        background-size: cover;
        height: 250px;
    }
</style>

<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12 mt-5">

                    <!-- Widget: user widget style 1 -->
                    <div class="card card-widget widget-user">
                        <!-- Add the bg color to the header using any of the bg-* classes -->
                        <div class="widget-user-header user-widget text-white"
                             style="background: url('img/cover1.jpg');
                              background-position: center center;
                               background-size: cover;
                                height: 250px;
                             ">

                            <h3 class="widget-user-username text-right">Elizabeth Pierce</h3>
                            <h5 class="widget-user-desc text-right">Web Designer</h5>
                        </div>
                        <div class="widget-user-image">
                            <img class="img-circle" :src="getProfilePhoto()" alt="User Avatar">
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



                    <div class="tab-pane active" id="settings">
                        <form class="form-horizontal">
                            <div class="form-group row">
                                <label for="name" class="col-sm-2 col-form-label">Name</label>
                                <div class="col-sm-10">
                                    <input  v-model="form.name"  type="name" class="form-control"
                                            id="name" placeholder="Name" :class="{ 'is-invalid':
                                                        form.errors.has('name') }">
                                    <has-error :form="form" field="name"></has-error>
                                </div>
                            </div>
                            <div class="form-group row">
                                <label for="email" class="col-sm-2 col-form-label">Email</label>
                                <div class="col-sm-10">
                                    <input v-model="form.email" type="email" class="form-control"
                                           id="email" placeholder="Email" :class="{ 'is-invalid':
                                                        form.errors.has('email') }" >
                                    <has-error :form="form" field="email"></has-error>
                                </div>
                            </div>

                            <div class="form-group row">
                                <label for="bio" class="col-sm-2 col-form-label">Experience</label>
                                <div class="col-sm-10">
                                    <textarea v-model="form.bio" class="form-control" id="bio"
                                              placeholder="Experience" :class="{ 'is-invalid':
                                                        form.errors.has('bio') }"></textarea>
                                         <has-error :form="form" field="bio"></has-error>
                                </div>
                            </div>
                            <div class="form-group row">
                                <label for="photo" class="col-sm-2 col-form-label">Profile Photo</label>
                                <div class="col-sm-10">
                                    <input type="file" class="form-control" @change="updateProfile" id="photo" placeholder="Upload Photo">
                                </div>
                            </div>

                            <div class="form-group row">
                                <label for="password" class="col-sm-2 col-form-label">Passport (Leave Empty if not changing)</label>
                                <div class="col-sm-10">
                                    <input type="password" v-model="form.password" class="form-control"
                                           id="password" placeholder="Passport">
                                </div>
                            </div>


                            <!--<div class="form-group row">
                                <div class="offset-sm-2 col-sm-10">
                                    <div class="checkbox">
                                        <label>
                                            <input type="checkbox"> I agree to the <a href="#">terms and conditions</a>
                                        </label>
                                    </div>
                                </div>
                            </div>-->
                            <div class="form-group row">
                                <div class="offset-sm-2 col-sm-10">
                                    <button type="submit"
                                      @click.prevent = "updateInfo"
                                      class="btn btn-success">Update</button>
                                </div>
                            </div>
                        </form>
                    </div>


                    <div class="tab-pane" id="activity">
                      <h2>Activity Settings</h2>
                    </div>
            </div>
                <!-- /.tab-content -->

        </div>
            <!-- /.card-body -->

    </div>

    </div>


</template>

<script>
    export default {

        data(){
             return {
                form: new Form({
                    id: '',
                    name : '',
                    email: '',
                    password: '',
                    type: '',
                    bio: '',
                    photo: '',

                })
            }
        },
        mounted() {
            console.log('Component mounted.')
        },

        methods: {

            getProfilePhoto(){
                let photo =  (this.form.photo.length > 100) ? this.form.photo: "img/profile/"+
                this.form.photo;
              return photo;
            },

            updateProfile(e){
                let file = e.target.files[0];
                console.log(file);
                let reader = new FileReader();

                let limit = 1024 * 1024  * 2;
                if(file['size'] < limit){
                reader.onloadend = (file) => {
                    this.form.photo = reader.result
                }
                reader.readAsDataURL(file);
                }
                else {
                    swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: 'You are uploading a large file',
                    })
                }
            },

            updateInfo(){
                this.$Progress.start();
                this.form.put('api/profile')
                    .then(() => {
                this.$Progress.finish();
                    })
                    .catch(() => {

                    }
                    );
            },
        },
        created() {
            axios.get('api/profile').then(({ data }) => (this.form.fill(data)))

        },
    }
</script>

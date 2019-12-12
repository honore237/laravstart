<template>
    <div class="container">
        <div class="row mt-5">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Users Management Table</h3>

                        <div class="card-tools">
                            <div class="btn btn-success" @click="newModal()">
                                Add New
                         <i class="fas fa-user-plus"></i>
                            </div>


                            <!-- Modal -->
                            <div class="modal fade" id="addNew" tabindex="-1" role="dialog" aria-labelledby="addNewLabel" aria-hidden="true">
                                <div class="modal-dialog modal-dialog-centered" role="document">
                                    <div class="modal-content">
                                        <div class="modal-header">
                                            <h5 class="modal-title" id="addNewLabel" v-show="!editMode">Add New </h5>
                                            <h5 class="modal-title" id="addNewLabel" v-show="editMode">Update User </h5>
                                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                                <span aria-hidden="true">&times;</span>
                                            </button>
                                        </div>


                                   <form @submit.prevent="editMode ? updateUser() : createUser()">

                                        <div class="modal-body">
                                            <div class="form-group">
                                                <input v-model="form.name" type="text" name="name"
                                                       placeholder = "Name"
                                                       class="form-control" :class="{ 'is-invalid':
                                                        form.errors.has('name') }">
                                                <has-error :form="form" field="name"></has-error>
                                            </div>

                                            <div class="form-group">
                                                <input v-model="form.email" type="email" name="email"
                                                       placeholder = "Email Address"
                                                       class="form-control" :class="{ 'is-invalid':
                                                        form.errors.has('email') }">
                                                <has-error :form="form" field="email"></has-error>
                                            </div>

                                            <div class="form-group">
                                                <input v-model="form.bio" id="bio" name="bio"
                                                       placeholder = "Short Bio user (Optional)"
                                                       class="form-control" :class="{ 'is-invalid':
                                                        form.errors.has('bio') }">
                                                <has-error :form="form" field="bio"></has-error>
                                            </div>


                                            <div class="form-group">
                                                <select v-model="form.type" name="type" id="type"
                                                        placeholder = "Type"
                                                        class="form-control" :class="{ 'is-invalid':
                                                        form.errors.has('type') }">
                                                    <option value="">Select User Role</option>
                                                    <option value="admin">Admin</option>
                                                    <option value="user">Standard</option>
                                                    <option value="author">Author</option>
                                                </select>
                                                <has-error :form="form" field="type"></has-error>
                                            </div>

                                            <div class="form-group">
                                                <input v-model="form.password" type="password" name="password"
                                                       placeholder = "Password"
                                                       class="form-control" :class="{ 'is-invalid':
                                                        form.errors.has('password') }">
                                                <has-error :form="form" field="password"></has-error>
                                            </div>

                                        </div>

                                        <div class="modal-footer">
                                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                                            <button  v-show="editMode" type="submit" class="btn btn-success">Update</button>
                                            <button v-show="!editMode" type="submit" class="btn btn-primary">Create</button>
                                        </div>

                                 </form>

                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- /.card-header -->
                    <div class="card-body table-responsive p-0">
                        <table class="table table-hover">
                            <thead>
                            <tr>
                                <th>Id</th>
                                <th>Name</th>
                                <th>Email</th>
                                <th>Role</th>
                                <th>Registered At</th>
                                <th>Actions</th>
                            </tr>
                            </thead>
                            <tbody>

                            <tr v-for="user in users" :key="user.id">
                                <td>{{user.id}}</td>
                                <td>{{user.name}}</td>
                                <td>{{user.email}}</td>
                                <td>{{user.type | upText}}</td>
                                <td>{{user.created_at | myDate}}</td>
                                <td>
                                    <a href="#"  @click="editUserModal(user)">
                                        <i class="fas fa-edit blue"></i>
                                    </a>
                                        /
                                    <a href="#" @click="deleteUser(user.id)">
                                        <i class="fas fa-trash red"></i>
                                    </a>
                                </td>
                            </tr>
                            </tbody>
                        </table>
                    </div>
                    <!-- /.card-body -->
                </div>
                <!-- /.card -->
            </div>
        </div>
    </div>
</template>

            <script>
                export default {
                    data(){
                        return {
                            editMode: false,
                            users : {},
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

                    methods : {

                        updateUser(){
                            //console.log('Updating Data');
                            this.$Progress.start();
                                this.form.put('api/users/'+this.form.id)
                                    .then(() => {

                                    })
                                    .catch(
                                        () => {
                                     this.$Progress.fail();
                                        }
                                    );
                        },
                        editUserModal(user){
                            this.editMode = true;
                            this.form.reset();
                            $('#addNew').modal('show');
                            this.form.fill(user);
                        },
                        newModal(){
                            this.editMode = false;
                            this.form.reset();
                            $('#addNew').modal('show')
                        },
                        deleteUser(id){
                            swal.fire({
                                title: 'Are you sure?',
                                text: "You won't be able to revert this!",
                                icon: 'warning',
                                showCancelButton: true,
                                confirmButtonColor: '#3085d6',
                                cancelButtonColor: '#d33',
                                confirmButtonText: 'Yes, delete it!'
                            }).then((result) => {

                                //send request to the server for delete
                                this.form.delete('api/users/'+id)
                                    .then(() => {
                                        if (result.value) {
                                                swal.fire(
                                                    'Deleted!',
                                                    'The User has been deleted successfully.',
                                                    'success'
                                              )
                                            Fire.$emit('AfterCreate');
                                        }
                                    }).catch(() => {
                                        swal.fire('Failed',
                                            'There Were something wrong',
                                            'warning')
                                });
                            })
                        },
                        loadUsers(){
                            axios.get('api/users').then(({data}) => (this.users = data.data));
                        },
                       createUser(){
                            this.$Progress.start();
                           // Submit the form via a POST request
                           /*this.form.post('api/user');*/
                           this.form.post('api/users')
                               .then(()=>{Fire.$emit('AfterCreate');
                                   //hide Modal
                                   $('#addNew').modal('hide')

                                   //toast
                                   toast.fire({
                                       icon: 'success',
                                       title: 'User Created successfully'
                                   })
                                   this.$Progress.finish();

                               })
                               .catch(()=>{

                               })

                       }
                    },
                    created() {
                       this.loadUsers();
                        Fire.$on('AfterCreate', () => {
                            this.loadUsers();
                        });
                       /*setInterval(() => this.loadUsers(), 3000);*/
                    }
                }
            </script>

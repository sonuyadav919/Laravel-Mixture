<template>
    <div>

    <div class="ibox float-e-margins">
        <div class="ibox-title">
            <h5>Update Company</h5>
            <div class="ibox-tools">
                <router-link to="/" class="btn btn-default btn-xs">Back</router-link>
            </div>
        </div>
        <div class="ibox-content">
            <div class="alert alert-danger" v-if="errors.length > 0">
                <ul>
                    <li v-for="error in errors">{{ error }}</li>
                </ul>
            </div>

            <form v-on:submit="saveForm()">
                <div class="row">
                    <div class="col-xs-12 form-group">
                        <label class="control-label">Company name</label>
                        <input type="text" v-model="company.name" class="form-control">
                    </div>
                </div>
                <div class="row">
                    <div class="col-xs-12 form-group">
                        <label class="control-label">Company address</label>
                        <input type="text" v-model="company.address" class="form-control">
                    </div>
                </div>
                <div class="row">
                    <div class="col-xs-12 form-group">
                        <label class="control-label">Company website</label>
                        <input type="text" v-model="company.website" class="form-control">
                    </div>
                </div>
                <div class="row">
                    <div class="col-xs-12 form-group">
                        <label class="control-label">Company email</label>
                        <input type="text" v-model="company.email" class="form-control">
                    </div>
                </div>
                <div class="row">
                    <div class="col-xs-12 form-group">
                        <button class="btn btn-success">Update</button>
                    </div>
                </div>
            </form>
        </div>
    </div>

    </div>
</template>

<script>
    export default {
        mounted() {
            let app = this;
            let id = app.$route.params.id;
            app.companyId = id;
            axios.get('/api/v1/companies/' + id)
                .then(function (resp) {
                    app.company = resp.data;
                })
                .catch(function () {
                    alert("Could not load your company")
                });
        },
        data: function () {
            return {
                companyId: null,
                company: {
                    name: '',
                    address: '',
                    website: '',
                    email: '',
                },
                errors: [],
            }
        },
        methods: {
            saveForm() {
                event.preventDefault();
                var app = this;
                var newCompany = app.company;
                axios.patch('/api/v1/companies/' + app.companyId, newCompany)
                    .then(function (resp) {
                        app.$router.replace('/');
                    })
                    .catch(error => {
                          this.errors = [];
                        if (error.response.data.errors.name) {
                            this.errors.push(error.response.data.errors.name[0]);
                        }

                        if (error.response.data.errors.address) {
                            this.errors.push(error.response.data.errors.address[0]);
                        }

                        if (error.response.data.errors.email) {
                            this.errors.push(error.response.data.errors.email[0]);
                        }

                        if (error.response.data.errors.website) {
                            this.errors.push(error.response.data.errors.website[0]);
                        }
                    });
            }
        }
    }
</script>

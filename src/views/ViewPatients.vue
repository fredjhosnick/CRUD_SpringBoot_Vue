<template>
    <main>
        <Navbar />

        <!--table-->

        <div class="container">
            <div class="row">
                <div class="col-md-12">
                    <h1 class="text-center">View Patients</h1>
                    <a href="/add" class="btn btn-primary">Add Patients</a>
                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th scope="col">Id</th>
                                <th scope="col">Name</th>
                                <th scope="col">Email</th>
                                <th scope="col">Phone</th>
                                <th scope="col">Gender</th>
                                <th scope="col">Actions</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="patient in patients" :key="patient.id">
                                <th scope="row">{{ patient.id }}</th>
                                <td>{{ patient.name }}</td>
                                <td>{{ patient.email }}</td>
                                <td>{{ patient.phone }}</td>
                                <td>{{ patient.gender }}</td>
                                <td>
                                    <a class="btn btn-primary" :href="`/edit/${patient.id}`">Edit</a>
                                    <button class="btn btn-danger mx-2"
                                        @click="deletePatients(patient.id)">Delete</button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

    </main>
</template>

<script>
import Navbar from '@/components/Navbar.vue';
export default {
    name: 'ViewPatients',
    components: {
        Navbar
    },
    data() {
        return {
            patients: []
        }
    },
    methods: {
        getPatients() {
            fetch('http://localhost:8080/patients')
                .then(res => res.json())
                .then(data => {
                    this.patients = data;
                    console.log(data);

                })
        },

        deletePatients(id) {
            fetch(`http://localhost:8080/patient/${id}`, {
                method: 'DELETE'
            })
                .then(data => {
                    if (data.ok) {
                        this.patients = this.patients.filter(patient => patient.id !== id)
                    } else {
                        console.log('Error to delete')
                    }
                })
                .catch(error => {
                    console.log('There is a problem', error);
                })
        }
    },
    beforeMount() {
        this.getPatients()
    }
}
</script>
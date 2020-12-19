<template>
    <div>

        <b-container class="bv-example-row">
            <b-row>
                <b-col>    <b-card class="mt-3" header="Register a Bike">




                    <b-form @submit="onSubmit" @reset="onReset" v-if="show">

                        <!-- Bike ID -->
                        <b-form-group
                        id="input-group-1"
                        label="Bike ID:"
                        label-for="input-1"
                        description="Enter the Bike ID (Integer)"
                        >
                        <b-form-input
                        id="input-1"
                        v-model="form.bike_id"
                        required
                        type="number"
                        placeholder="Enter Bike ID"
                        ></b-form-input>
                    </b-form-group>

                    <br>

                    <!-- Bike State  -->
                    <b-form-group
                    id="input-group-1"
                    label="Bike State:"
                    label-for="input-1"
                    description="Enter Bike State"
                    >
                    <b-form-input list="my-list-id"
                    v-model="form.state"
                    ></b-form-input>

                </b-form-group>


                <br>


                <b-row>
                    <b-col>
                        <!-- RFID ID -->
                        <b-form-group
                        id="input-group-1"
                        label="RFID ID:"
                        label-for="input-1"
                        description="Enter RFID Signature for the Bike"
                        >
                        <b-form-input
                        id="input-1"
                        v-model="form.rfid_code"
                        required
                        placeholder="Enter RFID Code"
                        ></b-form-input>


                    </b-form-group>
                </b-col>

                <b-col>

                    <!-- Bike Secret -->
                    <b-form-group
                    id="input-group-1"
                    label="Bike Bluetooth Secret:"
                    label-for="input-1"
                    description="Enter Bluetooth Secret for the Bike"
                    >
                    <b-form-input
                    id="input-1"
                    v-model="form.bike_secret"
                    required
                    type="number"
                    max="99999"
                    placeholder="Enter Bluetooth Secret"
                    ></b-form-input>


                </b-form-group>
            </b-col>


            <b-col>

                <!-- Dock ID -->
                <b-form-group
                id="input-group-1"
                label="Dock ID:"
                label-for="input-1"
                description="Enter Dock ID for the Bike"
                >
                <b-form-input
                id="input-1"
                v-model="form.dock_id"
                required
                type="number"
                step="any"
                placeholder="Enter Dock ID"
                ></b-form-input>


            </b-form-group>

        </b-col>


    </b-row>




    <b-form-group id="input-group-4">
        <b-form-checkbox-group v-model="form.checked" id="checkboxes-4">
            <b-form-checkbox value="Active">Active</b-form-checkbox>
        </b-form-checkbox-group>
    </b-form-group>



    <b-button type="submit" variant="primary">Register</b-button>
    <b-button type="reset" variant="danger">Reset</b-button>

</b-form>

</b-card>

</b-col>

<!-- Find and update a bike -->

<b-col>

    <b-card class="mt-3" header="Update or Delete Bike">


        <b-form @submit="onSubmitFind" @reset="onReset" v-if="show">

            <!-- Station ID -->
            <b-form-group
            id="input-group-1"
            label="Bike ID:"
            label-for="input-1"
            required
            description="Enter Bike ID to find the Bike"
            >
            <b-form-input
            id="input-1"
            v-model="bike_id"
            required
            type="number"
            placeholder="Enter Bike ID"
            ></b-form-input>
        </b-form-group>


        <b-button variant="primary" type="submit">Find</b-button>
        <br>
        <br>


    </b-form>

    <br>

    <div v-if="update">

        <b-list-group>

            <b-list-group-item href="#some-link"
            variant="success"
            >
            <b-row v-b-modal.modal-1>

                <b-col> Bike ID: {{bike.bike_id}} </b-col> <b-col> Bike State: {{bike.state}}</b-col>


            </b-row>

        </b-list-group-item>

        <b-modal id="modal-1" title="Dock Information">

            <b-container  class="bv-example-row">

                <b-row >

                    <b-col> <h5 align="center">Bike Details</h5> </b-col>

                </b-row>


                <b-row >
                    <form ref="form" @submit.stop.prevent="">
                        <b-row >

                            <b-col >
                                <h6> Bike ID </h6>
                            </b-col>

                            <b-col cols="8">

                                <b-form-input
                                id="name-input"
                                required
                                v-model="bike.bike_id"

                                ></b-form-input>

                            </b-col>

                        </b-row>

                        <b-row >

                            <b-col >
                                <h6> Bike State </h6>
                            </b-col>

                            <b-col cols="8">

                                <b-form-input list="my-list-id"
                                v-model="bike.state"
                                ></b-form-input>

                            </b-col>

                        </b-row>

                        <b-row >

                            <b-col >
                                <h6>  RFID ID </h6>
                            </b-col>

                            <b-col cols="8">

                                <b-form-input
                                id="name-input"
                                required
                                v-model="bike.rfid_code"


                                ></b-form-input>

                            </b-col>

                        </b-row>

                        <b-row >

                            <b-col >
                                <h6>  Bluetooth Secret </h6>
                            </b-col>

                            <b-col cols="8">

                                <b-form-input
                                id="name-input"
                                required
                                v-model="bike.bike_secret"
                                ></b-form-input>

                            </b-col>

                        </b-row>

                        <b-row >

                            <b-col >
                                <h6>  Dock ID </h6>
                            </b-col>

                            <b-col cols="8">

                                <b-form-input
                                id="name-input"
                                required
                                v-model="bike.dock_id"
                                ></b-form-input>

                            </b-col>

                        </b-row>

                    </form>

                </b-row>

                <!-- <div v-if="bike != null">
                <b-button type="reset" variant="success">Update</b-button>
                <b-button type="reset" variant="danger">Delete</b-button>
            </div> -->

        </b-container>

        <template v-slot:modal-footer="{ ok, cancel }">
            <!-- Emulate built in modal footer ok and cancel button actions -->
            <b-button size="sm" variant="success" @click="updateBike()">
                Update
            </b-button>
            <b-button size="sm" variant="danger" @click="cancel()">
                Cancel
            </b-button>
            <!-- Button with custom close trigger value -->
            <b-button size="sm" variant="outline-secondary" @click="deleteBike()">
                Delete
            </b-button>
        </template>
    </b-modal>
</b-list-group>

</div>

</b-card>

</b-col>


</b-row>
</b-container>

<datalist id="my-list-id">
    <option v-for="state in states"  :key="state.id" >{{ state }}</option>
</datalist>
</div>
</template>

<script>
export default {

    name: 'BikeForm',

    data() {
        return {
            form: {
                bike_id: null,
                state: '',
                rfid_code: '',
                bike_secret: '',
                dock_id:null,
                station_id_num:null,
            },
            bike_id: null,
            show: true,
            update:false,
            bike:null,
            states: [ 'AVAILABLE', 'CHARGING', 'UNAVAILABLE','LENDED']
        }
    },
    methods: {


        async updateBike() {

            this.$nextTick(() => {
                this.$bvModal.hide('modal-1')
            })

            this.update = false


            await this.$axios
            .post('api/auth/bike/update?bike_id=' + this.bike_id,this.bike)
            .then((res) => {
                console.log(res.data)
                this.bike = res.data
                this.resetForm()


                this.$notifier.showMessage({ content: `Bike Updated Successfully`, color: 'success' })
                // this.$router.push('login')
            })
            .catch((err) => {
                console.log(err.response)
                this.$notifier.showMessage({ content: `Error ${err.response.data.message}`, color: 'error' })
                this.resetForm()

                this.update = false

            })




        },



        async deleteBike() {

            this.$nextTick(() => {
                this.$bvModal.hide('modal-1')
            })

            this.update = false

            await this.$axios
            .delete('api/auth/bike/delete?bike_id=' + this.bike_id)
            .then((res) => {
                console.log(res.data)
                this.$notifier.showMessage({ content: `Bike was unregistered!`, color: 'danger' })
                this.resetForm()
                this.update = false


                //   this.$notifier.showMessage({ content: `Docks Found Successfully`, color: 'success' })
                // this.$router.push('login')
            })
            .catch((err) => {
                console.log(err.response)
                this.$notifier.showMessage({ content: `Error ${err.response.data.message}`, color: 'error' })
                this.resetForm()

                this.update = false

            })


        },

        async findBike() {

            await this.$axios
            .get('api/auth/bike/identify?bike_id=' + this.bike_id)
            .then((res) => {
                console.log(res.data)
                this.bike = res.data
                this.update = true
                this.resetForm()

                //   this.$notifier.showMessage({ content: `Docks Found Successfully`, color: 'success' })
                // this.$router.push('login')
            })
            .catch((err) => {
                console.log(err.response)
                this.$notifier.showMessage({ content: `Error ${err.response.data.message}`, color: 'error' })
                this.resetForm()

                this.update = false

            })


        },

        createBike(){
            this.$axios
            .post('api/auth/bike/register', this.form)
            .then((res) => {
                console.log(res.data)
                this.$notifier.showMessage({ content: `Bike Created Successfully`, color: 'success' })
                this.resetForm()
                // this.$router.push('login')
            })
            .catch((err) => {
                console.log(err.response)
                this.$notifier.showMessage({ content: `Error ${err.response.data.message}`, color: 'error' })
                this.resetForm()

            })

        },
        onSubmit(evt) {
            evt.preventDefault()
            this.createBike()
        },
        onSubmitFind(evt) {
            evt.preventDefault()
            this.findBike()

        },
        onReset(evt) {
            evt.preventDefault()
            // Reset our form values
            this.form.bike_id = null
            this.form.bike_secret = ''
            this.form.rfid_code = ''
            this.form.state = ''
            this.form.dock_id = ''
            this.bike = null

            // Trick to reset/clear native browser form validation state
            this.show = false
            this.$nextTick(() => {
                this.show = true
            })
        },
        resetForm() {
            // Reset our form values
            this.form.bike_id = null
            this.form.bike_secret = ''
            this.form.rfid_code = ''
            this.form.state = ''
            this.form.dock_id = ''

            // Trick to reset/clear native browser form validation state
            this.show = false
            this.$nextTick(() => {
                this.show = true
            })
        }
    }
}
</script>

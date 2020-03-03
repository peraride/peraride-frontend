<template>
  <div>

<b-container class="bv-example-row">

<b-row>
  <b-col cols="4">
    <b-card class="mt-3" header="Create a Dock">  
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">


        <!-- Station ID -->
        <b-form-group
        id="input-group-1"
        label="Station ID:"
        label-for="input-1"
        description="Enter Station ID"
      >
        <b-form-input
          id="input-1"
          v-model="form.station_id_num"
          required
          type="number"
          placeholder="Enter Station ID"
        ></b-form-input>

        
      </b-form-group>


        <!-- Dock ID -->
      <b-form-group
        id="input-group-1"
        label="Dock ID:"
        label-for="input-1"
        description="Enter the Dock ID corresponding to Station"
      >
      <b-form-input list="docks_ids" 
          v-model="form.dock_id"
          placeholder="Select ID"
       ></b-form-input>
      </b-form-group>
        <!-- Dock ID -->



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

  <b-col cols="8">


<!-- Dock Table -->
<b-card class="mt-3" header="Update or Delete Dock" style="text-align:center;">  



   <!-- User Interface controls -->
    <b-row>
      <b-col lg="6" class="my-1">
        <b-form-group
          label="Sort"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          label-for="sortBySelect"
          class="mb-0"
        >
          <b-input-group size="sm">
            <b-form-select v-model="sortBy" id="sortBySelect" :options="sortOptions" class="w-75">
              <template v-slot:first>
                <option value="">-- none --</option>
              </template>
            </b-form-select>
            <b-form-select v-model="sortDesc" size="sm" :disabled="!sortBy" class="w-25">
              <option :value="false">Asc</option>
              <option :value="true">Desc</option>
            </b-form-select>
          </b-input-group>
        </b-form-group>
      </b-col>

      <b-col lg="6" class="my-1">
        <b-form-group
          label="Initial sort"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          label-for="initialSortSelect"
          class="mb-0"
        >
          <b-form-select
            v-model="sortDirection"
            id="initialSortSelect"
            size="sm"
            :options="['asc', 'desc', 'last']"
          ></b-form-select>
        </b-form-group>
      </b-col>

      <b-col lg="6" class="my-1">
        <b-form-group
          label="Filter"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          label-for="filterInput"
          class="mb-0"
        >
          <b-input-group size="sm">
            <b-form-input
              v-model="filter"
              type="search"
              id="filterInput"
              placeholder="Type to Search"
            ></b-form-input>
            <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
      </b-col>

      <b-col lg="6" class="my-1">
        <b-form-group
          label="Filter On"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          description="Leave all unchecked to filter on all data"
          class="mb-0">
          <b-form-checkbox-group v-model="filterOn" class="mt-1">
            <b-form-checkbox value="name">Name</b-form-checkbox>
            <b-form-checkbox value="age">Age</b-form-checkbox>
            <b-form-checkbox value="isActive">Active</b-form-checkbox>
          </b-form-checkbox-group>
        </b-form-group>
      </b-col>

      <b-col sm="5" md="6" class="my-1">
        <b-form-group
          label="Per page"
          label-cols-sm="6"
          label-cols-md="4"
          label-cols-lg="3"
          label-align-sm="right"
          label-size="sm"
          label-for="perPageSelect"
          class="mb-0"
        >
          <b-form-select
            v-model="perPage"
            id="perPageSelect"
            size="sm"
            :options="pageOptions"
          ></b-form-select>
        </b-form-group>
      </b-col>

      <b-col sm="7" md="6" class="my-1">
        <b-pagination
          v-model="currentPage"
          :total-rows="totalRows"
          :per-page="perPage"
          align="fill"
          size="sm"
          class="my-0"
        ></b-pagination>
      </b-col>
    </b-row>

    <!-- Main table element -->
    <b-table
      show-empty
      small
      stacked="md"
      :items="docks"
      :fields="dockFields"
      :current-page="currentPage"
      :per-page="perPage"
      :filter="filter"
      :filterIncludedFields="filterOn"
      :sort-by.sync="sortBy"
      :sort-desc.sync="sortDesc"
      :sort-direction="sortDirection"
      @filtered="onFiltered"
    >
      <template v-slot:cell(dock_id)="row">
          <b-form-input style="border:0px;" v-model="row.item.dock_id"/>
        <!-- {{ row.value.first }} {{ row.value.last }} -->
      </template>

      <template v-slot:cell(dock_state)="row">
          <b-form-input style="border:0px;" v-model="row.item.dock_state" />
        <!-- {{ row.value.first }} {{ row.value.last }} -->
      </template>

        <template v-slot:cell(station_id_num)="row" >
          
          <b-form-input style="border:0px; text-align: center; margin: auto;" v-model="row.item.station_id_num"  type="number"  />
        <!-- {{ row.value.first }} {{ row.value.last }} -->
      </template>


        <template v-slot:cell(bike)="row" >
          <b-form-input style="border:0px; text-align: center; margin: auto;" v-model="row.item.bike"    />
        <!-- {{ row.value.first }} {{ row.value.last }} -->
      </template>

     
     
      <template v-slot:cell(actions)="row">
        <b-row>
        <b-button size="sm" variant="success" @click="updateDock(row.item)" class="mr-1">
         Update Dock
        </b-button>

        <b-button size="sm" variant="danger" @click="deleteDock(row.item)" class="mr-1">
          Remove Dock
        </b-button>

 
        </b-row>
      </template>

 
    </b-table>





 </b-card>

  </b-col>
</b-row>
</b-container>



<datalist id="docks_ids">
    <option v-for="dock in docks_ids"  :key="dock.id" >{{ dock }}</option>
  </datalist>

  </div>
</template>

<script>
  export default {
    
    name: 'DockForm',

    data() {
      return {
        color: 'success',
        selected: 0,
        form: {
          dock_id: null,
          button_id: null,
          station_id_num: null,
        },
        bike:null,
        show: true,
        update:false,
        docks:[],
        docks_ids: [ 1,2,3,4,5,6],
        station_id_num: null,
        curr_dock:null,
        bikeAvailable: false,
        docks:[],
        dockFields: [
          { key: 'station_id_num', label: 'Station ID', sortable: true, class: 'text-center' },
          { key: 'dock_id', label: 'Dock ID', sortable: true, sortDirection: 'desc' , class: 'text-center',},
          { key: 'dock_state', label: 'Dock State', sortable: true, sortDirection: 'desc' , class: 'text-center'},
          { key: 'bike', label: 'Bike', sortable: true, class: 'text-center' },

          { key: 'actions', label: 'Actions',      class: 'text-center' }
        ],
        totalRows: 1,
        currentPage: 1,
        perPage: 5,
        pageOptions: [5, 10, 15],
        sortBy: '',
        sortDesc: false,
        sortDirection: 'asc',
        filter: null,
        states: [true,false],
        filterOn: [],
        infoModal: {
          id: 'info-modal',
          title: '',
          content: ''
        },
        stations:[]

      }
    },
    
  
    computed: {
      sortOptions() {
        // Create an options list from our fields
        return this.dockFields
          .filter(f => f.sortable)
          .map(f => {
            return { text: f.label, value: f.key }
          })
      }
    },
    mounted() {
      // Set the initial number of items
      this.totalRows = this.docks.length
    
        this.loadDocks()

    },

    methods: {

    isSelected (dock) {
      if(dock.dock_state == 'LOCKED')
      return 'success'
      if(dock.dock_state == 'FREE')
      return 'warning'
      if(dock.dock_state == 'UNAVAILABLE')
      return 'danger'


    },
      renderDock(dock){

        // console.log(dock.bike)
        if(dock.bike.length != 0){
          this.bike = dock.bike
          this.curr_dock = dock
          this.bikeAvailable = true
        }else{
          this.bikeAvailable = false
        }

      },

      async findDock() {

          await this.$axios
					.get('api/auth/dock/locate?station_id=' + this.station_id_num)
					.then((res) => {
                        // console.log(res.data)
                        this.docks = res.data.docks

                        // console.log(this.docks.bike[0])


                        // if(res.data.docks.bike)
                        // this.bikeAvailable = true
                        this.update = true


    			    //   this.$notifier.showMessage({ content: `Docks Found Successfully`, color: 'success' })
				        // this.$router.push('login')
					})
					.catch((err) => {
						console.log(err.response)
    			      this.$notifier.showMessage({ content: `Error ${err.response.data.message}`, color: 'error' })

					})

      },

      createDock(){

         this.form.dock_id =  parseFloat(this.form.station_id_num) + this.form.dock_id/10
         this.form.button_id =  parseFloat(this.form.station_id_num) + this.form.button_id/10

          this.$axios
					.post('api/auth/dock/register', this.form)
					.then((res) => {
						console.log(res.data)
                this.$notifier.showMessage({ content: `Dock Created Successfully`, color: 'success' })
                
				        // this.$router.push('login')
					})
					.catch((err) => {
						console.log(err.response)
    			      this.$notifier.showMessage({ content: `Error ${err.response.data.message}`, color: 'error' })

					})

      },
      info(item, index, button) {
        this.infoModal.title = `Row index: ${index}`
        this.infoModal.content = JSON.stringify(item, null, 2)
        this.$root.$emit('bv::show::modal', this.infoModal.id, button)
      },
      resetInfoModal() {
        this.infoModal.title = ''
        this.infoModal.content = ''
      },
      onFiltered(filteredItems) {
        // Trigger pagination to update the number of buttons/pages due to filtering
        this.totalRows = filteredItems.length
        this.currentPage = 1
      },
      onSubmit(evt) {
        evt.preventDefault()
        this.createDock()
      },
      onSubmitFind(evt) {
        evt.preventDefault()
        this.findDock()

      },
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.form.dock_id = ''
        this.form.button_id = ''
        this.form.station_id_num = null
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      },

       async updateDock(dockObj) {

            console.log(dockObj)
            

          await this.$axios
					.post('api/dock/update?dock_id=' + dockObj.dock_id,dockObj)
					.then((res) => {

            this.loadDocks()

            this.$notifier.showMessage({ content: `User Updated Successfully`, color: 'success' })
            
				        // this.$router.push('login')
					})
					.catch((err) => {
						console.log(err.response)
                      this.$notifier.showMessage({ content: `Error ${err.response.data.message}`, color: 'error' })
                      

          })
          

         
      },

      
      async loadDocks() {


          await  this.$axios
					.get('api/auth/station/locateAll' )
					.then((res) => {
                        this.stations = res.data.stations
                        // console.log(this.users)
                        this.docks = []

                        this.stations.forEach(element => {

                          element.docks.forEach(element => {

                            console.log(element.bike.length)

                            if(element.bike.length == 0)
                              this.docks.push({ dock_id: element.dock_id, dock_state: element.dock_state, station_id_num: element.station_id_num})
                            else{
                                   this.docks.push({ dock_id: element.dock_id, dock_state: element.dock_state, station_id_num: element.station_id_num,
                                   bike: element.bike[0].bike_id            }) 
                            }   
                              
                          });


                      // this.docks.push({ isActive: element.active, age: element.age, role: element.role,
                      // name: { first: element.name, last: '' },
                      // mobile:element.mobile , email:element.email , _rowVariant: (element.active ? '' : 'danger')})      


                    });


					})
					.catch((err) => {
						console.log(err.response)
                      this.$notifier.showMessage({ content: `Error ${err.response.data.message}`, color: 'error' })

          })
          

         
      },        
        
      async deleteUser(userObj) {


          await this.$axios
					.delete('api/auth/delete?email=' + userObj.email)
					.then((res) => {
                        console.log(res.data)
    			              this.$notifier.showMessage({ content: `User was unregistered!`, color: 'danger' })
                          this.loadUsers()


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
    }
  }
</script>
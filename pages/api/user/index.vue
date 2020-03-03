<template>
<div class="back-ground">



 <no-ssr>
    <l-map ref="map" class="mini-map" :zoom="zoom" :center="center" >




    <l-tile-layer :url="url"></l-tile-layer>


      <l-control :position="'bottomright'">
        <b-button variant="danger" @click="locateMe" >Locate Me</b-button>

      </l-control>

      <l-control :position="'topright'" v-if="gettingLocation"> 

			<v-overlay >
			<v-btn
				icon
				@click="overlay = false"
			>
			<b-row>
				<b-spinner variant="success" type="grow" style="width: 6rem; height: 6rem;"  label="Text Centered"></b-spinner>
			</b-row>
			</v-btn>
			</v-overlay>

		<div class="text-center">
		</div>

      </l-control>


 <!-- Markers for Station Locations -->

	<l-marker
        v-for="marker in markers"
        :key="marker.id"
        :visible="marker.visible"
  		:icon="icon"
        :draggable="marker.draggable"
        :lat-lng.sync="marker.position"
      > 
		 <l-popup>
			 <div v-if="details"  style="width: 100%; height: 100%;" > <div>
									<h3> {{marker.id}} </h3>
				 					<h4> Station Status: ACTIVE</h4>
									<h5> Available Bikes </h5>
									  <div v-for="bike in marker.bike"  :key="bike.bike_id"  >
										<li  >

								         Bike No: {{bike.bike_id}} Docked At: {{bike.dock_id}}
										</li> 
									  </div>

									<br>
									<h5> Available Free Docks </h5>
									  <div v-for="dock in marker.docks"  :key="dock.dock_id"  >
										<li v-if="dock.dock_state == 'FREE'" >
											{{dock.dock_id}}
								        	{{dock.bike.bike_id}}
											
										</li> 
									  </div>
									<br>
							
						 <b-button variant="danger" @click="greet(marker.id)">Go Back</b-button> </div>
							</div> 
						 

			 <div v-else>	 
				<div>
					<h4> {{marker.id}} </h4>
					<h6>
					<b-button variant="success" @click="greet(marker.id)">Check Station</b-button>
					</h6>
				</div>
			</div>

		  
        </l-popup>
	   </l-marker>

 <!-- Marker for User Location -->
		<l-marker
		:lat-lng.sync="center"
		:draggable="false"
     
      > 
	  <l-popup :content="`<h6>Hi ${this.$auth.user.name} </h6>`" />
	  </l-marker>

  </l-map>
 </no-ssr>



</div>

<!-- 
  <div v-if="location">
    Your location data is {{ location.coords.latitude }}, {{ location.coords.longitude}}
  </div> -->
				<!-- <button @click="locateMe">Locate Me</button> -->

	
</template>

<script>

import LoginForm from '../../../components/LoginForm'
import Test from '../../../components/test'

	export default {

		components: {
			// Navbar,
			LoginForm
		},
		middleware: 'auth-user',
	
		data: () => ({

			draggable: true,
			overlay:true,
			popupContent: "Sentian HQ",
			zoom: 17,
			center: [55.607741796855734, 13.018133640289308],
			marker: [55.607741796855734, 13.018133640289308],
			iconSize: [55, 55],
			location: null,
			gettingLocation: false,
			errorStr: null,
			map:null,
			stations: [],
			details: false,
			icon:null,
			url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
			markerLatLng: [47.313220, -1.319482],
			
		    markers: [
				{
				id: 'm1',
				position: { lat: 51.505, lng: -0.09 },
				tooltip: 'tooltip for marker1',
				draggable: true,
				visible: true,
				station: null

				},
				{
				id: 'm2',
				position: { lat: 51.8905, lng: -0.09 },
				tooltip: 'tooltip for marker2',
				draggable: true,
				visible: false,
				}

			],
	
		}),


		mounted() {

	
			//do we support geolocation
			if (!("geolocation" in navigator)) {
			this.errorStr = "Geolocation is not available.";
			
			return;
			}
			// get position

			this.gettingLocation = true;
			navigator.geolocation.getCurrentPosition(pos => {
			this.gettingLocation = false;
			this.location = pos;
			this.center = [pos.coords.latitude,pos.coords.longitude]
			this.marker = [pos.coords.latitude,pos.coords.longitude]


			}, err => {
			this.gettingLocation = false;
			this.errorStr = err.message;
			})


			this.icon =  L.icon({
				iconUrl: '/bike_icon.png',
				// shadowUrl: '/user_icon_shadow.png',
				iconSize:     [45, 45], // size of the icon

			});		
			
			this.getStation()

			setInterval(() => {

				if(this.$auth.loggedIn)
				this.getStation()
			}, 10000);

		},

		methods: {


			    getStation(){
				     this.$axios
					.get('api/auth/station/locateAll')
					.then((res) => {

						// this.stations = res.data.docks
						// console.log(res.data.stations)

						res.data.stations.forEach((element) => {

						var marker = {
										id: element.station_name,
										position: { lat: element.latitude, lng: element.longitude },
										tooltip: 'tooltip for marker2',
										draggable: false,
										visible: true,
										station: element,
										docks: [],
										bike: []
									 }	

									element.docks.forEach((element) => {
										marker.docks.push(element)
										// markers.bike.push()

											element.bike.forEach((element) => {

												marker.bike.push(element)

											
											});



									});


						var foundIndex = this.markers.findIndex(x => x.id == marker.id);	
						if(foundIndex > 0)
						this.markers[foundIndex] = marker;
						else{
						this.markers.push(marker)
						}
						
						});

				        // this.$router.push('login')
					})
					.catch((err) => {
						console.log(err.response)
					})

				},


				addMarker(lat,lon,html){


				},
			
			
				async getLocation() {
				
				return new Promise((resolve, reject) => {

					if(!("geolocation" in navigator)) {
					reject(new Error('Geolocation is not available.'));
					}

					navigator.geolocation.getCurrentPosition(pos => {
					resolve(pos);
					}, err => {
					reject(err);
					});

				});
				},

			greet: function(station){
				this.details = !this.details
				this.getStation() // Gives an error
			},


			locateMe: async function() {


				try {
					console.log(this.location)

					this.map.setView(L.latLng(this.location.coords.latitude,this.location.coords.longitude), 20)
			this.gettingLocation = false;


				} catch(e) {
					this.errorStr = e.message;
					console.log(this.errorStr)

				}
				
				}

		},

		updated() {
			//do we support geolocation
			if (!("geolocation" in navigator)) {
			this.errorStr = "Geolocation is not available.";
			return;
			}
			navigator.geolocation.getCurrentPosition(pos => {
			this.location = pos;
			this.center = [pos.coords.latitude,pos.coords.longitude]
			this.marker = [pos.coords.latitude,pos.coords.longitude]


			}, err => {
			this.errorStr = err.message;
			}
			);

			const mapComponent = this.$refs.map
			const map = mapComponent.mapObject // the leaflet map object
			this.map = map

		}

};
</script>


<style scoped>

.mini-map {
  width: 100%;
  height: 800px;
}
.star{
	background-color: #4CAF50; /* Green */
	background: red;
	color: #000;
  font-size: 4.5em;
    border-radius: 4px;
	  height: 26px; 
  width: 26px;  
} 

.custom-control {
  background: #fff;
  padding: 0 0.5em;
  border: 1px solid #aaa;
  border-radius: 0.6em;
}

</style>
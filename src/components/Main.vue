<template>
	<div>
		<div class="inputs">
			<label for="">Meal:</label>
			<input type="text" v-model="food" placeholder="enter food">
			<label for="">Location:</label>
			<input type="text" v-model="location" placeholder="enter location">
			<button class="btn" @click="getData(food, location)">Search</button>
		</div>
		<div class="featured-wrap">
			<h2 v-show="featuredRest" class="featured">Featured: {{featuredRest}}</h2>
			<div class="pix" v-for="photo in photos" :key="photo.id">
				 <h4>{{photo.user.firstName}}</h4>
				<!-- Name: {{photo.source.url}} -->
				<img :src="photo.prefix+'100x100'+photo.suffix" />
			</div>
		</div>
		<div class="wrapper">
			<div v-for="venue in venues" :key="venue.id" class="restaurant">
				<p><strong>{{venue.name}}</strong></p>
				<p>{{venue.location.address}},</p>
				<p>{{venue.location.city}},  {{venue.location.state}}</p>
				<p>{{venue.contact.formattedPhone}}</p>
				<a v-show="venue.contact.facebook" :href="'https://www.facebook.com/'+venue.contact.facebook" target="_blank">Visit us on facebook</a>
			</div>
		</div>
	</div>
</template>

<script>
	import axios from 'axios'
	import api_keys from '../../server_keys'

	const clientID=api_keys.client_id
	const clientSecret=api_keys.client_secret

	export default{
		data(){
			return {
				venues: [],
				loading: false,
				fetchError: null,
				food: '',
				location: '',
				photos: [],
				venueId: '',
				featuredRest: ''
			}
		},
		methods:  {
			getData: function(food, location){
				this.location = location;
				this.food = food;
				this.loading=true;

				// distance is only available with ll (lat & long)
				// 40.722983971416696,-73.99453401203763
				// ll requires lat and longtitude instead of zip
				// minivenues
				// axios.get("https://api.foursquare.com/v2/venues/suggestcompletion?ll="+this.location+"&query="+this.food+"&client_id="+clientID+"&client_secret="+clientSecret)

				// &limit=1
				//get restaurant and then get photo from response
				//"near" can take zipcode, lat and longtitude or name
				axios.get("https://api.foursquare.com/v2/venues/search?near="+this.location+"&query="+this.food+"&client_id="+clientID+"&client_secret="+clientSecret)
				.then((response) => { 
					this.venues=response.data.response.venues
					//set first one as featured with an image
					this.featuredRest=response.data.response.venues[0].name
					let venueID = response.data.response.venues[0].id
					return axios.get("https://api.foursquare.com/v2/venues/"+venueID+"/photos?group=venue&client_id="+clientID+"&client_secret="+clientSecret)
				})
				.then((response) => {
					this.photos=response.data.response.photos.items
				})
				.catch(error => {
			        this.fetchError = error
			    })
			} //getData
		} //methods
	}
</script>

<style lang="scss" scoped>
.featured-wrap {
	display: flex;
	flex-wrap: wrap;

	.featured {
		display: block;
	    width: 100%;
	    margin: 10px auto;
	    text-align: center;
	}
}

.pix {
	margin: 10px 10px;
    flex: 0 1 auto;
    align-self: flex-start;
    padding: 10px;
    background: rgba(azure, 0.75);
	h4 {
		margin:10px 0;
		padding: 0
	}
}

.inputs{
	margin: 0 auto;
    text-align: center;
}

.wrapper{
	display: flex;
	flex-wrap: wrap;

	.restaurant{
		flex: 0 1 auto;
		align-self: flex-start; //auto | flex-end | center | baseline | stretch
		border: 1px solid #f1f1f1;
		box-sizing: border-box;
		font-weight: normal;
		padding: 20px;
	}
	.restaurant:first-of-type{
		width: 100%;
	    display: inline-block;
	    background: azure;
	    font-size: 1.5em;
	    text-align: center;
	}
	.restaurant p{
		word-wrap: break-word;
	    word-break: break-word;
	    margin: 10px 0;
	}
	
}

.btn{
	margin: 20px 0;
	padding: 5px;
	cursor:pointer;
}

a{
	color: cadetblue;
	text-decoration: none;

	&:hover, &:focus{
		color: lighten(cadetblue, 20%);
	}
}

@media all and (min-width: 300px){
	.restaurant{
		min-width: 100%
	}
}
@media all and (min-width: 768px){
	.restaurant{
		min-width: 49.9%;
		width: 49.9%;
	}
}
@media all and (min-width: 1024px){
	.restaurant{
		min-width: 33.3%;
		width: 33.3%;
	}
}
</style>

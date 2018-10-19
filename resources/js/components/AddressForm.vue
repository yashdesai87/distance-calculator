<template>
    <div class="container">
        <div class="row mb-2">
            <h1>Distance Calculator</h1>
        </div>
        <div class="row justify-content-center mt-10">
            <div class="col-md-5">
                <form>
                    <div class="form-row">
                            <label for="inputEmail4">Start typing an address. Address 1</label>
                        <gmap-autocomplete
                            class="form-control"
                            @place_changed="setPlace1">
                        </gmap-autocomplete>
                    </div>
                    <hr>
                    <div class="form-row">
                        <div class="form-group col-md-6">
                            <label for="inputEmail4">Street Address</label>
                            <input type="text" name="address" placeholder="Street Address" class="form-control" :value=" street_address_1 ? street_address_1: ''">
                        </div>
                        <div class="form-group col-md-6">
                            <label for="inputPassword4">City</label>
                            <input type="text" name="city" placeholder="City" class="form-control" :value="city_1 ? city_1: ''">
                        </div>
                    </div>
                    <div class="form-row">
                        <div class="form-group col-md-6">
                            <label for="inputEmail4">State</label>
                            <input type="text" name="state" placeholder="State" class="form-control" :value="state_1 ? state_1  : ''">
                        </div>
                        <div class="form-group col-md-6">
                            <label for="inputPassword4">Zip</label>
                            <input type="text" name="zip" placeholder="Zip" class="form-control" :value="zip_1 ? zip_1: ''">
                        </div>
                    </div>
                </form>
            </div>
            <div class="col-md-2"></div>

            <div class="col-md-5">
                <form>
                    <div class="form-row">
                        <label for="inputEmail4">Start typing an address. Address 2</label>
                        <gmap-autocomplete
                            class="form-control"
                            @place_changed="setPlace2">
                        </gmap-autocomplete>
                    </div>
                    <hr>
                    <div class="form-row">
                        <div class="form-group col-md-6">
                            <label for="inputEmail4">Street Address</label>
                            <input type="text" name="address" placeholder="Street Address" class="form-control" :value=" street_address_2 ? street_address_2: ''">
                        </div>
                        <div class="form-group col-md-6">
                            <label for="inputPassword4">City</label>
                            <input type="text" name="city" placeholder="City" class="form-control" :value="city_2 ? city_2: ''">
                        </div>
                    </div>
                    <div class="form-row">
                        <div class="form-group col-md-6">
                            <label for="inputEmail4">State</label>
                            <input type="text" name="state" placeholder="State" class="form-control" :value="state_2 ? state_2  : ''">
                        </div>
                        <div class="form-group col-md-6">
                            <label for="inputPassword4">Zip</label>
                            <input type="text" name="zip" placeholder="Zip" class="form-control" :value="zip_2 ? zip_2: ''">
                        </div>
                    </div>
                </form>
            </div>
        </div>
        <div class="row mt-2">
            <div class="col-md-12 text-center">
                <button type="button" @click.prevent="calculate_distance" class="btn btn-primary">Calculate Distance</button>
            </div>
        </div>
    </div>
</template>

<script>
import * as VueGoogleMaps from "vue2-google-maps";

    export default {
        data() {
            return {
                street_address_1: '',
                street_address_2: '',
                address_1: null,
                address_2: null,
                city_1: null,
                city_2: null,
                state_1: null,
                state_2: null,
                zip_1:null,
                zip_2:null,
                lat_1: null,    
                long_1: null,    
                lat_2: null,    
                long_2: null,
                distance: 0,    
            };
        },


        methods: {
                
            setPlace1(place) {
              
              // Get the values from the places object
                this.street_address_1 = place['formatted_address'];
               this.address_1 = _.get(place, 'address_components');
                let _this = this;
                _.forEach(this.address_1, function (component) {
                    let types = _.get(component, 'types');
                    if (_.includes(types, 'locality')) {
                        _this.city_1 = _.get(component, 'long_name');
                    }

                    if (_.includes(types, 'administrative_area_level_1')) {
                        _this.state_1 = _.get(component, 'long_name');
                    }

                    if (_.includes(types, 'postal_code')) {
                        _this.zip_1 = _.get(component, 'long_name');
                    }
                });

                this.lat_1 = place.geometry.location.lat();
                this.long_1 = place.geometry.location.lng();
            },

             setPlace2(place) {
                
                // Get the values from the places object
                this.street_address_2 = place['formatted_address'];
               this.address_2 = _.get(place, 'address_components');
                let _this = this;
                _.forEach(this.address_2, function (component) {
                    let types = _.get(component, 'types');
                    if (_.includes(types, 'locality')) {
                        _this.city_2 = _.get(component, 'long_name');
                    }

                    if (_.includes(types, 'administrative_area_level_1')) {
                        _this.state_2 = _.get(component, 'long_name');
                    }

                    if (_.includes(types, 'postal_code')) {
                        _this.zip_2 = _.get(component, 'long_name');
                    }
                });

                this.lat_2 = place.geometry.location.lat();
                this.long_2 = place.geometry.location.lng();
            },

            calculate_distance(){
               
               // Used Haversine algorithm to calculate the distance
               const haversine = require('haversine')

                let start = {
                    latitude: this.lat_1,
                    longitude: this.long_1
                }

                let end = {
                    latitude: this.lat_2,
                    longitude: this.long_2
                }


               alert('The distance is '+ haversine(start, end,{unit: 'mile'}).toFixed(2)+' miles');

                }
            }
		
    }
</script>

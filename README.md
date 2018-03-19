# Restaurant App built in VueJS using data from the Foursquare API. Enter meal and location using zipcode, name or lat and long. The first call for venue endpoint data nests a second call to the photos endpoint. 

Substitute your own Foursquare client_id and secret.

Webpack and vue-loader added.

HtmlWebpackPlugin and CopyWebpackPlugin added.

Configured with the following loaders: 
- babel
- sass, css, and style
- fonts 
- images
- vue-loader


### Installation

1. Clone or download the repository:

```
$ git clone https://github.com/ameliapower/vue-restaurants
``` 

2. Go to the root of the project and install all project's dependencies:
```
$ npm install
```

3. Run npm server:
```
$ npm start
```

4. Run build:
```
$ npm run-script build
```
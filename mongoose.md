
# Mongoode Nodejs 

## Model Defination

### Movie Model
```javascript
const MovieSchema = new Schema({
  originalTitle: {   
    type: String,
    required: true
  },
  movieDescription: {  
    type: String,
    required: true
  },
  releaseDate: {    
    type: Date,
  },
  originalLanguage: {  
    type: String
  },      
  genres: {   
    type: Array
  },
  tags : {
    type: Array
  },
  category : {
    type: String
  },
  subcategory : {
    type: String
  },  
  movieUrl: {
    type: String
  },
  trailerUrls: {
    type: Array
  },
  cbfcRating: {
    type: String
  }, 
  duration: {
    type: String
  },
  averageRating: {
    type: Number
  },  
  viewCount: {
    type: Number,
  }   
  price:{
    type: Number,
    default : 0
  }, 
  cast: {
    type: Array
  },
  producer: {
    type: Object
  },
  status:{
    type: String   //Released ot not
  },
  editorsChoice: {
    type: Boolean,
    default : false
  },   
  enable: {
    type: Boolean,
    default : false //Active or Disabled
  }, 
  highlight: {
    type: Boolean,
    default : false //Active or Disabled
  },   
  uploadedBy: {
    type: Object,
  },
  createdOn: {
    type: Date,
    default: Date.now
  }
});
module.exports = mongoose.model("movie", MovieSchema);
```
### User Model
```javascript
const AccountSchema = new Schema({
  username: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true
  },
  password: {
    type: String
  },
  phone: {
    type: String
  },  
  dateOfRegistration: {
    type: Date,
    default: Date.now
  },
  createdOn: {
    type: Date,
    default: Date.now
  },
  moviesBought: {
    type: Array,
  },
  moviesWishlist: {
    type: Array,
  },
  subscribedPlan: {
    type: String,
  },
  languages: {
    type: Array,
  },
  location: {
    type: Object,
  },
  registeredDevices: {
    type: Array,
  },
  moviesWatched: {
    type: Array,
  },
});

module.exports = mongoose.model("account", AccountSchema);
```
### Orders Model
```javascript
const OrderSchema = new Schema({
  accountId : { 
    type: Schema.Types.ObjectId, 
    ref: 'accounts' 
  },
  movieId: {
    type: Schema.Types.ObjectId,
    ref: "movies"
  },
  orderId: {
    type: String,
    required: true
  },
  orderDescription: {
    type: String    
  },
  amount: {
    type: Number,
    required: true
  },        
  paymentDone: {
    type: Boolean,
    default: false    
  },       
  orderTimestamp: {
    type: Date,
    default: Date.now
  },      
});

module.exports = mongoose.model("order", OrderSchema);
```
## CRUD
### Create
```javascript
var user = {
    username: "",
    email:"",
    password:"",
    phone:"",
}
let newUser = new User(user );
newUser.save()
  .then( function (producer) { console.log("Saved") } )
  .catch( function (err) { console.log(err) } )
```
### Read
```javascript
var conditions = { email: { $eq: "user@gmail.com" } }
// Get one record
User.findOne(conditions).exec()
  .then( function (user) { console.log(user) })
  .catch( function (err) { console.log(err) }  )

// Get multiple records
User.find(conditions).exec()
  .then( function (users) { console.log(users) })
  .catch( function (err) { console.log(err) }  )

// Get multiple records and select username and email
User.find(conditions).select({ "username": 1,"email": 1, "_id": 0}).exec()
  .then( function (users) { console.log(users) })
  .catch( function (err) { console.log(err) }  )
```
### Update
```javascript
var condition = { username : "user2"}
var user = {
    email : "user@yahoo.com"
    }
    
User.updateOne(condition, user)
  .then( function (result) { 
      var res = if result.nModified==0 ? "No Updates" : "Updated"
      console.log(res)
   })
  .catch( function (err) { console.log(err) }  )
  
User.updateMany(condition , user)
```
### Delete
```javascript
var condition = { username : "user2"}
User.remove(condition)
  .then( function (result) { 
      var res = if result.nModified==0 ? "User Not found" : "Deleted"
      console.log(res)
   })
  .catch( function (err) { console.log(err) }  )
```
## Filtering

```javascript
  var q = "user"
  per_page = 10
  page_no = 1

  limit: per_page ,
  skip:per_page * (page_no - 1)
  
 conditions = {
     $or: [
         { name: { $regex: new RegExp('/^'+q+'$/i') }  },
         { name: { $regex: new RegExp(q),$options : 'i' }  }
         ]
    }
 Artist.find({ $text: { $search: q } }).skip(pagination.skip).limit(pagination.limit).exec()
   .then( function (users) { console.log(users) })
   .catch( function (err) { console.log(err) }  )
```
### with mongoose-paginate-v2
```javascript
const mongoosePaginate = require('mongoose-paginate-v2');
const AccountSchema = new Schema({
  username: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true
  },
  password: {
    type: String
  },
}

AccountSchema .plugin(mongoosePaginate);

module.exports = mongoose.model("Account", AccountSchema );   

var q = "user"
  
const options = {
    page: 1,
    limit: 10,
    collation: {
      locale: 'en'
    }
  };
 conditions = {
     $or: [
         { name: { $regex: new RegExp('/^'+q+'$/i') }  },
         { name: { $regex: new RegExp(q),$options : 'i' }  }
         ]
    }
Movie.paginate(condition, options)
   .then( function (result) { console.log(result) })
   .catch( function (err) { console.log(err) }  )

//      details = {
//       "movies":result.docs,
//        "options":{
//          "totalDocs":result.totalDocs,
//          "limit":result.limit,
//          "page":result.page,
//          "totalPages":result.totalPages,
//          "hasNextPage":result.hasNextPage,
//          "nextPage":result.nextPage,
//          "hasPrevPage":result.hasPrevPage,
//         "prevPage":result.prevPage,
//          "pagingCounter":result.pagingCounter          
//        }
//      } 
```

## Aggregation
### Count

```javascript
Movie.aggregate( [		// Count all Movies
        { $count: "movies" }
    ] 
  )   // Count all

Movie.aggregate( [             // Count with with country as india
      { $match: {  country: "india" }  },
      { $count: "user" }
  ] )   

movieId = "1"
var match = { "movieId":  mongoose.Types.ObjectId(movieId ) }
Order.aggregate([ 
        { $match : match  },
        { 
           _id:"$movieId",
           revenue : { $sum : $amount },
           count: { $sum:1 }
        } 
        ]}  

```

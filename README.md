# AbhiBus

### user Schema

``` javascript
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  Name: {
    type: String,
    required: true,
    default: "",
  },
  emailId: {
    type: String,
    required: true,
    default: "",
  },
  password: {
    type: String,
    required: true,
    default: "",
  },
  phoneNumber: {
    type: String,
    required: true,
    default: "",
  },
  gender: {
    type: String,
    enum: ["male", "female"],
  },
  userType: {
    type: String,
    enum: ["user", "admin"],
    default: "user",
  },
});

const User = mongoose.model("User", userSchema);

module.exports = User;
```
### Bus route Schema

``` javascript

const mongoose = require("mongoose");

const routeSchema = new mongoose.Schema({
  Pickup: {
    type: String,
    enum: [
      "Delhi",
      "Mumbai",
      "Bangalore",
      "Pune",
      "Hyderabad",
      "Trivendrum",
      "Kochi",
      "Patna",
      "Shimla",
      "Sikkim",
      "Ranchi",
    ],
    required: true,
    default: "",
  },
  destination: {
    type: String,
    enum: [
      "Delhi",
      "Mumbai",
      "Bangalore",
      "Pune",
      "Hyderabad",
      "Trivendrum",
      "Kochi",
      "Patna",
      "Shimla",
      "Sikkim",
      "Ranchi",
    ],
    required: true,
    default: "",
  },
  Time: {
    type: Number,
    required: true,
    default: "",
  },
  Long: {
    type: Number,
  },
  Lat: {
    type: Number,
  },
});

module.exports = mongoose.model("Route", routeSchema);
```
### BusSchema 

``` javascript
const mongoose = require("mongoose");

const busSchema = new mongoose.Schema({
  TypeOfBus: {
    type: String,
    enum: ["AC", "Non-Ac", "Sleeper", "Luxury"],
    required: true,
  },
  Name: {
    type: String,
    required: true,
  },
  BusNumber: {
    type: String,
    required: true,
  },
  NoOfSeats: {
    type: Number,
    required: true,
  },
  TypeOfSeats: {
    type: String,
    enum: ["male", "female", "single", "double"],
    required: true,
  },
  BoardingPoints: {
    type: String,
    required: true,
  },
  droppingPoints: {
    type: String,
    required: true,
  },
  Amenities: {
    type: String,
    enum: [
      "Seat belt",
      "Wi-fi",
      "child seating",
      "Recliners",
      "Power outlets",
      "Private entrance",
      "Emergency exit",
    ],
    default: "",
  },
  status: {
    type: String,
    enum: ["on-time", "delayed", "cancelled"],
    default: "on-time",
  },
  DepartureTime: {
    type: String,
    enum: ["Before 6 am", "6 am to 12 pm", "12pm to 6 am", "After 6pm"],
    default: "",
  },
  ArrivalTime: {
    type: String,
    enum: ["Before 6 am", "6 am to 12 pm", "12pm to 6 am", "After 6pm"],
    default: "",
  },
  price: {
    type: Number,
    required: true,
  },
});

module.exports = mongoose.model("Bus", busSchema);
```

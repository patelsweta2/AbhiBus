``` javascript


const stopPoint = {
      stopId:{
        type: Number,
        required:true,
      },
      title: {
        type: String,
        required: true,
      },
      directions: {
        type: String,
        required: true,
      },
      lat: {
        type: Number,
      },
      lng: {
        type: Number,
      },
    },




// City Schema
const citySchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
  },
  state: {
    type: String,
    required: true,
  },
  lat: {
    type: Number,
  },
  lng: {
    type: Number,
  },
  stopPoints: [
    stopPoint
  ],
  pinCode:{
    type:Number,
    required:true,
  }
});

//

const busTypes={
    AC:"AC",
    SLEEPER: "SLEEPER",
    SEATER:"SEATER",
    NON_AC:"NON_AC"
}

const amenities = ["WIFI","LIVE_TRACKING","CHARGING_PORT","EMERGENCY_EXIT","WATER"]


const SeatTypes = [ "SEATER", "SLEEPER" ]

const Seat = {
            seatNumber: {type: String, required: true},
            row: { type: Number, required: true},
            column: { type: Number, required: true },
            type: { type: String, enum: SeatTypes, required: true }
        }

// Bus Schema
const busSchema = new mongoose.Schema({
  busPartner: {
    type: String,
    required: [true, "Bus name is required"],
  },
  busType: {
    type: String,
    enum: [busTypes.AC,busTypes.SLEEPER,busTypes.SEATER,busTypes.NON_AC],
    required: [true, "Bus Type is required"],
  },
  amenities: {
    type: String,
    enum:amenities,
    default: "",
  },
  busNumber: {
    type: String,
    required: [true, "License no. is required"],
  },
  layout:{
    upperDeck:{type:[Seat]},
    lowerDeck:{type:[Seat],required:true},
  }
});



// trip schema
const tripSchema = new mongoose.Schema({
    source:{
        ref: 'Citie',
        type: mongoose.Schema.type.ObjectId,
    },
    destination:{
        ref:'Citie',
        type: mongoose.Schema.type.ObjectId
    },
    boardingPoints:[
        {
            stopId:{type:Number,required:true},
            arrivalTime:{type:Number,required:true}               //use epoch time
        }
    ],
    droppingPoints:[
        {
            stopId:{type:Number,required:true},
            arrivalTime:{type:Number,required:true}               //use epoch time
        }
    ],
    prices:[
        {
            seatNumber:{type:String,required:true},
            price:{type:Number,required:true}
        }
    ],
    startTime:{type:Number,required:true},
    endTime:{type:Number,required:true},
    driverDetails:{
        contactNumber:{type:String,required:true},
        name:{type:String,required:true},
    }
})

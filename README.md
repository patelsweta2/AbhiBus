### Schema for AbhiBus

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
```
### Dummy Data for bus

``` javascript
const generateSeats = (rows, columns, type) => {
    const seats = []; 
    for (let row = 1; row <= rows; row++) {
      for (let col = 1; col <= columns; col++) {
        seats.push({
          seatNumber: `${row}${String.fromCharCode(64 + col)}`,
          type : type  
        });
      }
    }
    return seats;
  };

const buses = [
    {
        busPartner: "olaBus", 
        busType: ["AC", "SLEEPER", "SEATER"], 
        amenities: ["WiFi", "Charging Point"],
        busNumber: 'DL52RS5475',
        layout: {
            lowerDeck : generateSeats(6, 4, 'SEATER'), 
            upperDeck: generateSeats(3, 3, 'SLEEPER')  
        }
 
    }, 
    {
        busPartner: "NEOGOBUS", 
        busType: ["AC", "SEATER"], 
        amenities: ["Charging Point", "EMERGENCY_EXIT", "WATER"],
        busNumber: '51695452',
        layout: {
            lowerDeck : generateSeats(10, 6, 'SEATER'), 
            // upperDeck: generateSeats(3, 3, 'SLEEPER')  
        }
 
    }, 
    {
        busPartner: "UBERGO", 
        busType: ["AC", "SEATER"], 
        amenities: ["Charging Point", "EMERGENCY_EXIT", "WATER"],
        busNumber: '62458746',
        layout: {
            lowerDeck : generateSeats(5, 4, 'SLEEPER'), 
            // upperDeck: generateSeats(3, 3, 'SLEEPER')  
        }
 
    }, 
    {
        busPartner: "IRCTC", 
        busType: ["AC", "SLEEPER"],  
        amenities: ["Charging Point", "EMERGENCY_EXIT", "WATER", "FOOD"],
        busNumber: '45815668',
        layout: {
            lowerDeck : generateSeats(5, 4, 'SLEEPER'), 
            upperDeck: generateSeats(5, 4, 'SLEEPER')  
        }
 
    }, 
    {
        busPartner: "GOVT BUS SERVICES", 
        busType: ["NON_AC", "SEATER" ],  
        amenities: [""],
        busNumber: '956482',
        layout: {
            lowerDeck : generateSeats(10, 6, 'SEATER'), 
            // upperDeck: generateSeats(5, 4, 'SLEEPER')  
        }
 
    },
      {
        busPartner: "KSRTC", 
        busType: ["AC", "SEATER" ],  
        amenities: [""],
        busNumber: '4561235',
        layout: {
            lowerDeck : generateSeats(10, 6, 'SEATER'), 
            // upperDeck: generateSeats(5, 4, 'SLEEPER')  
        }
 
    },
      {
        busPartner: "GSRTC", 
        busType: ["AC", "SLEEPER", "SETAER" ],  
        amenities: ["WiFi","Charging Point", "Water", "Food"],
        busNumber: '7427036',
        layout: {
            lowerDeck : generateSeats(6, 4, 'SEATER'), 
             upperDeck: generateSeats(3, 3, 'SLEEPER')  
        }
 
    },
      {
        busPartner: "OSRTC", 
        busType: ["NON_AC", "SLEEPER", "SEATER" ],  
        amenities: ["WATER", "Charging Point"],
        busNumber: '9345189',
        layout: {
            lowerDeck : generateSeats(5, 4, 'SEATER'), 
            upperDeck: generateSeats(5, 4, 'SLEEPER')  
        }
 
    },
      {
        busPartner: "Orange Tours and Travels", 
        busType: ["NON_AC", "SLEEPER" ],  
        amenities: ["Charging Point","WiFi"],
        busNumber: '98236781',
        layout: {
            lowerDeck : generateSeats(5, 4, 'SEATER'), 
            upperDeck: generateSeats(5, 4, 'SLEEPER')  
        }
 
    },
      {
        busPartner: "zingbus plus", 
        busType: ["NON_AC", "SLEEPER" ],  
        amenities: ["Charging Point", "Water", "Food"],
        busNumber: '2358918',
        layout: {
            lowerDeck : generateSeats(6, 4, 'SEATER'), 
             upperDeck: generateSeats(5, 4, 'SLEEPER')  
        }
 
    }, 
 
]

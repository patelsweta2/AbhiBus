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
```
### Trip data

``` javascript
[
  {
    "source": "64c8e5d3fc13ae5e50a53df4",
    "destination": "64c8e5d3fc13ae5e50a53df6",
    "boardingPoints": [
      {"stopId": 101, "arrivalTime": 1693113600},
      {"stopId": 102, "arrivalTime": 1693117200},
      {"stopId": 103, "arrivalTime": 1693120800}
    ],
    "droppingPoints": [
      {"stopId": 201, "arrivalTime": 1693135200},
      {"stopId": 202, "arrivalTime": 1693138800},
      {"stopId": 203, "arrivalTime": 1693142400}
    ],
    "prices": [
      {"seatNumber": "1A", "price": 1200},  // SLEEPER
      {"seatNumber": "1B", "price": 1200},  // SLEEPER
      {"seatNumber": "1C", "price": 1000},  // SEATER
      {"seatNumber": "1D", "price": 1000}   // SEATER
    ],
    "startTime": 1693106400,
    "endTime": 1693149600,
    "driverDetails": {
      "contactNumber": "9876543210",
      "name": "Rajesh Kumar"
    }
  },
  {
    "source": "64c8e5d3fc13ae5e50a53df7",
    "destination": "64c8e5d3fc13ae5e50a53df8",
    "boardingPoints": [
      {"stopId": 104, "arrivalTime": 1693200000},
      {"stopId": 105, "arrivalTime": 1693203600},
      {"stopId": 106, "arrivalTime": 1693207200}
    ],
    "droppingPoints": [
      {"stopId": 204, "arrivalTime": 1693221600},
      {"stopId": 205, "arrivalTime": 1693225200},
      {"stopId": 206, "arrivalTime": 1693228800}
    ],
    "prices": [
      {"seatNumber": "2A", "price": 1300},  // SLEEPER
      {"seatNumber": "2B", "price": 1300},  // SLEEPER
      {"seatNumber": "2C", "price": 1100},  // SEATER
      {"seatNumber": "2D", "price": 1100}   // SEATER
    ],
    "startTime": 1693192800,
    "endTime": 1693236000,
    "driverDetails": {
      "contactNumber": "9123456789",
      "name": "Arvind Singh"
    }
  },
  {
    "source": "64c8e5d3fc13ae5e50a53df9",
    "destination": "64c8e5d3fc13ae5e50a53dfa",
    "boardingPoints": [
      {"stopId": 107, "arrivalTime": 1693286400},
      {"stopId": 108, "arrivalTime": 1693290000},
      {"stopId": 109, "arrivalTime": 1693293600}
    ],
    "droppingPoints": [
      {"stopId": 207, "arrivalTime": 1693308000},
      {"stopId": 208, "arrivalTime": 1693311600},
      {"stopId": 209, "arrivalTime": 1693315200}
    ],
    "prices": [
      {"seatNumber": "3A", "price": 1400},  // SLEEPER
      {"seatNumber": "3B", "price": 1400},  // SLEEPER
      {"seatNumber": "3C", "price": 1200},  // SEATER
      {"seatNumber": "3D", "price": 1200}   // SEATER
    ],
    "startTime": 1693279200,
    "endTime": 1693322400,
    "driverDetails": {
      "contactNumber": "9988776655",
      "name": "Sunil Sharma"
    }
  },
  {
    "source": "64c8e5d3fc13ae5e50a53dfb",
    "destination": "64c8e5d3fc13ae5e50a53dfc",
    "boardingPoints": [
      {"stopId": 110, "arrivalTime": 1693372800},
      {"stopId": 111, "arrivalTime": 1693376400},
      {"stopId": 112, "arrivalTime": 1693380000}
    ],
    "droppingPoints": [
      {"stopId": 210, "arrivalTime": 1693394400},
      {"stopId": 211, "arrivalTime": 1693398000},
      {"stopId": 212, "arrivalTime": 1693401600}
    ],
    "prices": [
      {"seatNumber": "4A", "price": 1500},  // SLEEPER
      {"seatNumber": "4B", "price": 1500},  // SLEEPER
      {"seatNumber": "4C", "price": 1300},  // SEATER
      {"seatNumber": "4D", "price": 1300}   // SEATER
    ],
    "startTime": 1693365600,
    "endTime": 1693408800,
    "driverDetails": {
      "contactNumber": "8877665544",
      "name": "Amit Patel"
    }
  },
  {
    "source": "64c8e5d3fc13ae5e50a53dfd",
    "destination": "64c8e5d3fc13ae5e50a53dfe",
    "boardingPoints": [
      {"stopId": 113, "arrivalTime": 1693459200},
      {"stopId": 114, "arrivalTime": 1693462800},
      {"stopId": 115, "arrivalTime": 1693466400}
    ],
    "droppingPoints": [
      {"stopId": 213, "arrivalTime": 1693480800},
      {"stopId": 214, "arrivalTime": 1693484400},
      {"stopId": 215, "arrivalTime": 1693488000}
    ],
    "prices": [
      {"seatNumber": "5A", "price": 1600},  // SLEEPER
      {"seatNumber": "5B", "price": 1600},  // SLEEPER
      {"seatNumber": "5C", "price": 1400},  // SEATER
      {"seatNumber": "5D", "price": 1400}   // SEATER
    ],
    "startTime": 1693452000,
    "endTime": 1693495200,
    "driverDetails": {
      "contactNumber": "7766554433",
      "name": "Vikram Chauhan"
    }
  }
]
```

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
      {"stopId": 1, "arrivalTime": 1693113600},
      {"stopId": 5, "arrivalTime": 1693117200},
      {"stopId": 2, "arrivalTime": 1693120800}
    ],
    "droppingPoints": [
      {"stopId": 9, "arrivalTime": 1693135200},
      {"stopId": 8, "arrivalTime": 1693138800},
      {"stopId": 3, "arrivalTime": 1693142400}
    ],
     "prices":[
            {
                "seatNumber":"A6",
                "price":1499
            },
            {
                "seatNumber":"B6",
                "price":1499
            },
            {
                "seatNumber":"C6",
                "price":1499
            },
            {
                "seatNumber":"D6",
                "price":1499
            },
            {
                "seatNumber":"E6",
                "price":1499
            },
            {
                "seatNumber":"F6",
                "price":1499
            },
            {
                "seatNumber":"A4",
                "price":1499
            },
            {
                "seatNumber":"B4",
                "price":1499
            },
            {
                "seatNumber":"C4",
                "price":1499
            },
            {
                "seatNumber":"D4",
                "price":1499
            },
            {
                "seatNumber":"E4",
                "price":1499
            },
            {
                "seatNumber":"F4",
                "price":1499
            },
            {
                "seatNumber":"A2",
                "price":1899
            },
            {
                "seatNumber":"B2",
                "price":1899
            },
            {
                "seatNumber":"C2",
                "price":1899
            },
            {
                "seatNumber":"D2",
                "price":1899
            },
            {
                "seatNumber":"E2",
                "price":1899
            },
            {
                "seatNumber":"F2",
                "price":1899
            },
            {
                "seatNumber":"A5",
                "price":1899
            },
            {
                "seatNumber":"B5",
                "price":1899
            },
            {
                "seatNumber":"C5",
                "price":1899
            },
            {
                "seatNumber":"D5",
                "price":1899
            },
            {
                "seatNumber":"E5",
                "price":1899
            },
            {
                "seatNumber":"F5",
                "price":1899
            },
            {
                "seatNumber":"A3",
                "price":1899
            },
            {
                "seatNumber":"B3",
                "price":1899
            },
            {
                "seatNumber":"C3",
                "price":1499
            },
            {
                "seatNumber":"D3",
                "price":1499
            },
            {
                "seatNumber":"E3",
                "price":1499
            },
            {
                "seatNumber":"F3",
                "price":1499
            },
            {
                "seatNumber":"A1",
                "price":1899
            },
            {
                "seatNumber":"B1",
                "price":1899
            },
            {
                "seatNumber":"C1",
                "price":1899
            },
            {
                "seatNumber":"D1",
                "price":1899
            },
            {
                "seatNumber":"E1",
                "price":1899
            },
            {
                "seatNumber":"F1",
                "price":1899
            }
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
      {"stopId": 23, "arrivalTime": 1693200000},
      {"stopId": 45, "arrivalTime": 1693203600},
      {"stopId": 21, "arrivalTime": 1693207200}
    ],
    "droppingPoints": [
      {"stopId": 26, "arrivalTime": 1693221600},
      {"stopId": 27, "arrivalTime": 1693225200},
      {"stopId": 28, "arrivalTime": 1693228800}
    ],
     "prices":[
            {
                "seatNumber":"A6",
                "price":1499
            },
            {
                "seatNumber":"B6",
                "price":1499
            },
            {
                "seatNumber":"C6",
                "price":1499
            },
            {
                "seatNumber":"D6",
                "price":1499
            },
            {
                "seatNumber":"E6",
                "price":1499
            },
            {
                "seatNumber":"F6",
                "price":1499
            },
            {
                "seatNumber":"A4",
                "price":1499
            },
            {
                "seatNumber":"B4",
                "price":1499
            },
            {
                "seatNumber":"C4",
                "price":1499
            },
            {
                "seatNumber":"D4",
                "price":1499
            },
            {
                "seatNumber":"E4",
                "price":1499
            },
            {
                "seatNumber":"F4",
                "price":1499
            },
            {
                "seatNumber":"A2",
                "price":1899
            },
            {
                "seatNumber":"B2",
                "price":1899
            },
            {
                "seatNumber":"C2",
                "price":1899
            },
            {
                "seatNumber":"D2",
                "price":1899
            },
            {
                "seatNumber":"E2",
                "price":1899
            },
            {
                "seatNumber":"F2",
                "price":1899
            },
            {
                "seatNumber":"A5",
                "price":1899
            },
            {
                "seatNumber":"B5",
                "price":1899
            },
            {
                "seatNumber":"C5",
                "price":1899
            },
            {
                "seatNumber":"D5",
                "price":1899
            },
            {
                "seatNumber":"E5",
                "price":1899
            },
            {
                "seatNumber":"F5",
                "price":1899
            },
            {
                "seatNumber":"A3",
                "price":1899
            },
            {
                "seatNumber":"B3",
                "price":1899
            },
            {
                "seatNumber":"C3",
                "price":1499
            },
            {
                "seatNumber":"D3",
                "price":1499
            },
            {
                "seatNumber":"E3",
                "price":1499
            },
            {
                "seatNumber":"F3",
                "price":1499
            },
            {
                "seatNumber":"A1",
                "price":1899
            },
            {
                "seatNumber":"B1",
                "price":1899
            },
            {
                "seatNumber":"C1",
                "price":1899
            },
            {
                "seatNumber":"D1",
                "price":1899
            },
            {
                "seatNumber":"E1",
                "price":1899
            },
            {
                "seatNumber":"F1",
                "price":1899
            }
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
      {"stopId": 12, "arrivalTime": 1693286400},
      {"stopId": 13, "arrivalTime": 1693290000},
      {"stopId": 14, "arrivalTime": 1693293600}
    ],
    "droppingPoints": [
      {"stopId": 15, "arrivalTime": 1693308000},
      {"stopId": 16, "arrivalTime": 1693311600},
      {"stopId": 17, "arrivalTime": 1693315200}
    ],
     "prices":[
            {
                "seatNumber":"A6",
                "price":1200
            },
            {
                "seatNumber":"B6",
                "price":1200
            },
            {
                "seatNumber":"C6",
                "price":1200
            },
            {
                "seatNumber":"D6",
                "price":1200
            },
            {
                "seatNumber":"E6",
                "price":1200
            },
            {
                "seatNumber":"F6",
                "price":1200
            },
            {
                "seatNumber":"A4",
                "price":1200
            },
            {
                "seatNumber":"B4",
                "price":1690
            },
            {
                "seatNumber":"C4",
                "price":1200
            },
            {
                "seatNumber":"D4",
                "price":1200
            },
            {
                "seatNumber":"E4",
                "price":1500
            },
            {
                "seatNumber":"F4",
                "price":1500
            },
            {
                "seatNumber":"A2",
                "price":1500
            },
            {
                "seatNumber":"B2",
                "price":1500
            },
            {
                "seatNumber":"C2",
                "price":1500
            },
            {
                "seatNumber":"D2",
                "price":1500
            },
            {
                "seatNumber":"E2",
                "price":1500
            },
            {
                "seatNumber":"F2",
                "price":1500
            },
            {
                "seatNumber":"A5",
                "price":1500
            },
            {
                "seatNumber":"B5",
                "price":1500
            },
            {
                "seatNumber":"C5",
                "price":1790
            },
            {
                "seatNumber":"D5",
                "price":1500
            },
            {
                "seatNumber":"E5",
                "price":1500
            },
            {
                "seatNumber":"F5",
                "price":1500
            },
            {
                "seatNumber":"A3",
                "price":1500
            },
            {
                "seatNumber":"B3",
                "price":1500
            },
            {
                "seatNumber":"C3",
                "price":1500
            },
            {
                "seatNumber":"D3",
                "price":1500
            },
            {
                "seatNumber":"E3",
                "price":1500
            },
            {
                "seatNumber":"F3",
                "price":1500
            },
            {
                "seatNumber":"A1",
                "price":1500
            },
            {
                "seatNumber":"B1",
                "price":1500
            },
            {
                "seatNumber":"C1",
                "price":1890
            },
            {
                "seatNumber":"D1",
                "price":1500
            },
            {
                "seatNumber":"E1",
                "price":1500
            },
            {
                "seatNumber":"F1",
                "price":1500
            }
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
      {"stopId": 18, "arrivalTime": 1693372800},
      {"stopId": 19, "arrivalTime": 1693376400},
      {"stopId": 20, "arrivalTime": 1693380000}
    ],
    "droppingPoints": [
      {"stopId": 31, "arrivalTime": 1693394400},
      {"stopId": 32, "arrivalTime": 1693398000},
      {"stopId": 33, "arrivalTime": 1693401600}
    ],
     "prices":[
            {
                "seatNumber":"A6",
                "price":1000
            },
            {
                "seatNumber":"B6",
                "price":1000
            },
            {
                "seatNumber":"C6",
                "price":1000
            },
            {
                "seatNumber":"D6",
                "price":1690
            },
            {
                "seatNumber":"E6",
                "price":1000
            },
            {
                "seatNumber":"F6",
                "price":1000
            },
            {
                "seatNumber":"A4",
                "price":1690
            },
            {
                "seatNumber":"B4",
                "price":1000
            },
            {
                "seatNumber":"C4",
                "price":1000
            },
            {
                "seatNumber":"D4",
                "price":1000
            },
            {
                "seatNumber":"E4",
                "price":1000
            },
            {
                "seatNumber":"F4",
                "price":1000
            },
            {
                "seatNumber":"A2",
                "price":1400
            },
            {
                "seatNumber":"B2",
                "price":1400
            },
            {
                "seatNumber":"C2",
                "price":1400
            },
            {
                "seatNumber":"D2",
                "price":1400
            },
            {
                "seatNumber":"E2",
                "price":1400
            },
            {
                "seatNumber":"F2",
                "price":1400
            },
            {
                "seatNumber":"A5",
                "price":1200
            },
            {
                "seatNumber":"B5",
                "price":1200
            },
            {
                "seatNumber":"C5",
                "price":1200
            },
            {
                "seatNumber":"D5",
                "price":1200
            },
            {
                "seatNumber":"E5",
                "price":1200
            },
            {
                "seatNumber":"F5",
                "price":1200
            },
            {
                "seatNumber":"A3",
                "price":1200
            },
            {
                "seatNumber":"B3",
                "price":1200
            },
            {
                "seatNumber":"C3",
                "price":1200
            },
            {
                "seatNumber":"D3",
                "price":1790
            },
            {
                "seatNumber":"E3",
                "price":1200
            },
            {
                "seatNumber":"F3",
                "price":1200
            },
            {
                "seatNumber":"A1",
                "price":1400
            },
            {
                "seatNumber":"B1",
                "price":1400
            },
            {
                "seatNumber":"C1",
                "price":1400
            },
            {
                "seatNumber":"D1",
                "price":1400
            },
            {
                "seatNumber":"E1",
                "price":1400
            },
            {
                "seatNumber":"F1",
                "price":1400
            }
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
      {"stopId": 34, "arrivalTime": 1693459200},
      {"stopId": 35, "arrivalTime": 1693462800},
      {"stopId": 36, "arrivalTime": 1693466400}
    ],
    "droppingPoints": [
      {"stopId": 37, "arrivalTime": 1693480800},
      {"stopId": 38, "arrivalTime": 1693484400},
      {"stopId": 39, "arrivalTime": 1693488000}
    ],
     "prices":[
            {
                "seatNumber":"A6",
                "price":1400
            },
            {
                "seatNumber":"B6",
                "price":1400
            },
            {
                "seatNumber":"C6",
                "price":1400
            },
            {
                "seatNumber":"D6",
                "price":1400
            },
            {
                "seatNumber":"E6",
                "price":1400
            },
            {
                "seatNumber":"F6",
                "price":1400
            },
            {
                "seatNumber":"A4",
                "price":1400
            },
            {
                "seatNumber":"B4",
                "price":1400
            },
            {
                "seatNumber":"C4",
                "price":1400
            },
            {
                "seatNumber":"D4",
                "price":1400
            },
            {
                "seatNumber":"E4",
                "price":1400
            },
            {
                "seatNumber":"F4",
                "price":1400
            },
            {
                "seatNumber":"A2",
                "price":1800
            },
            {
                "seatNumber":"B2",
                "price":1800
            },
            {
                "seatNumber":"C2",
                "price":1800
            },
            {
                "seatNumber":"D2",
                "price":1800
            },
            {
                "seatNumber":"E2",
                "price":1800
            },
            {
                "seatNumber":"F2",
                "price":1800
            },
            {
                "seatNumber":"A5",
                "price":1400
            },
            {
                "seatNumber":"B5",
                "price":1790
            },
            {
                "seatNumber":"C5",
                "price":1400
            },
            {
                "seatNumber":"D5",
                "price":1400
            },
            {
                "seatNumber":"E5",
                "price":1400
            },
            {
                "seatNumber":"F5",
                "price":1400
            },
            {
                "seatNumber":"A3",
                "price":1400
            },
            {
                "seatNumber":"B3",
                "price":1400
            },
            {
                "seatNumber":"C3",
                "price":1400
            },
            {
                "seatNumber":"D3",
                "price":1790
            },
            {
                "seatNumber":"E3",
                "price":1400
            },
            {
                "seatNumber":"F3",
                "price":1400
            },
            {
                "seatNumber":"A1",
                "price": 1800
            },
            {
                "seatNumber":"B1",
                "price":1800
            },
            {
                "seatNumber":"C1",
                "price":1800
            },
            {
                "seatNumber":"D1",
                "price":1800
            },
            {
                "seatNumber":"E1",
                "price":1800
            },
            {
                "seatNumber":"F1",
                "price":1800
            }
            ],
    "startTime": 1693452000,
    "endTime": 1693495200,
    "driverDetails": {
      "contactNumber": "7766554433",
      "name": "Vikram Chauhan"
    }
  },
{
        "busId":"63ac03fcd233f5c2b3a24b34",
        "source":"64fa03fcd233f5c2b3a24b34",
        "destination":"74fa03fcd233f5c2b3a24b34",
        "boardingPoints":[
            {
                "stopId":1,
                "arrivalTime":1724238000
            },
            {
                "stopId":2,
                "arrivalTime":1724241600
            },
            {
                "stopId":3,
                "arrivalTime":1724243400
            },
            {
                "stopId":4,
                "arrivalTime":1724245200
            },
            {
                "stopId":5,
                "arrivalTime":1724249700
            },
            {
                "stopId":6,
                "arrivalTime":1724255100
            },
            {
                "stopId":7,
                "arrivalTime":1724257200
            },
            {
                "stopId":8,
                "arrivalTime":1724261400
            }
        ],
        "droppingPoints":[
            {
                "stopId":4,
                "arrivalTime":1724245200
            },
            {
                "stopId":5,
                "arrivalTime":1724249700
            },
            {
                "stopId":6,
                "arrivalTime":1724255100
            },
            {
                "stopId":7,
                "arrivalTime":1724257200
            },
            {
                "stopId":8,
                "arrivalTime":1724261400
            },
            {
                "stopId":9,
                "arrivalTime":1724263200
            },
            {
                "stopId":10,
                "arrivalTime":1724264400
            },
            {
                "stopId":11,
                "arrivalTime":1724265900
            }
        ],
        "prices":[
            {
                "seatNumber":"A6",
                "price":1690
            },
            {
                "seatNumber":"B6",
                "price":1690
            },
            {
                "seatNumber":"C6",
                "price":1690
            },
            {
                "seatNumber":"D6",
                "price":1690
            },
            {
                "seatNumber":"E6",
                "price":1690
            },
            {
                "seatNumber":"F6",
                "price":1690
            },
            {
                "seatNumber":"A4",
                "price":1690
            },
            {
                "seatNumber":"B4",
                "price":1690
            },
            {
                "seatNumber":"C4",
                "price":1690
            },
            {
                "seatNumber":"D4",
                "price":1690
            },
            {
                "seatNumber":"E4",
                "price":1690
            },
            {
                "seatNumber":"F4",
                "price":1690
            },
            {
                "seatNumber":"A2",
                "price":1890
            },
            {
                "seatNumber":"B2",
                "price":1890
            },
            {
                "seatNumber":"C2",
                "price":1890
            },
            {
                "seatNumber":"D2",
                "price":1890
            },
            {
                "seatNumber":"E2",
                "price":1890
            },
            {
                "seatNumber":"F2",
                "price":1890
            },
            {
                "seatNumber":"A5",
                "price":1790
            },
            {
                "seatNumber":"B5",
                "price":1790
            },
            {
                "seatNumber":"C5",
                "price":1790
            },
            {
                "seatNumber":"D5",
                "price":1790
            },
            {
                "seatNumber":"E5",
                "price":1790
            },
            {
                "seatNumber":"F5",
                "price":1790
            },
            {
                "seatNumber":"A3",
                "price":1790
            },
            {
                "seatNumber":"B3",
                "price":1790
            },
            {
                "seatNumber":"C3",
                "price":1790
            },
            {
                "seatNumber":"D3",
                "price":1790
            },
            {
                "seatNumber":"E3",
                "price":1790
            },
            {
                "seatNumber":"F3",
                "price":1790
            },
            {
                "seatNumber":"A1",
                "price":1890
            },
            {
                "seatNumber":"B1",
                "price":1890
            },
            {
                "seatNumber":"C1",
                "price":1890
            },
            {
                "seatNumber":"D1",
                "price":1890
            },
            {
                "seatNumber":"E1",
                "price":1890
            },
            {
                "seatNumber":"F1",
                "price":1890
            }
        ],
        "startTime":1724238000,
        "endTime":1724338070,
        "driverDetails":{
            "contactNumber":"7609554167",
            "name":"Ramesh Kumar"
        }
    },



    {
        "busId":"72ac03fcd233f5c2b3a24b34",
        "source":"64fa03fcd233f5c2b3a24b33",
        "destination":"78fa03fcd233f5c2b3a24b34",
        "boardingPoints":[
            {
                "stopId":11,
                "arrivalTime":1724238000
            },
            {
                "stopId":12,
                "arrivalTime":1724241600
            },
            {
                "stopId":13,
                "arrivalTime":1724243400
            },
            {
                "stopId":14,
                "arrivalTime":1724245200
            },
            {
                "stopId":15,
                "arrivalTime":1724249700
            },
            {
                "stopId":16,
                "arrivalTime":1724255100
            },
            {
                "stopId":17,
                "arrivalTime":1724257200
            },
            {
                "stopId":18,
                "arrivalTime":1724261400
            }
        ],
        "droppingPoints":[
            {
                "stopId":14,
                "arrivalTime":1724245200
            },
            {
                "stopId":15,
                "arrivalTime":1724249700
            },
            {
                "stopId":16,
                "arrivalTime":1724255100
            },
            {
                "stopId":17,
                "arrivalTime":1724257200
            },
            {
                "stopId":18,
                "arrivalTime":1724261400
            },
            {
                "stopId":19,
                "arrivalTime":1724263200
            },
            {
                "stopId":20,
                "arrivalTime":1724264400
            },
            {
                "stopId":21,
                "arrivalTime":1724265900
            }
        ],
        "prices":[
            {
                "seatNumber":"A6",
                "price":1190
            },
            {
                "seatNumber":"B6",
                "price":1190
            },
            {
                "seatNumber":"C6",
                "price":1190
            },
            {
                "seatNumber":"D6",
                "price":1190
            },
            {
                "seatNumber":"E6",
                "price":1190
            },
            {
                "seatNumber":"F6",
                "price":1190
            },
            {
                "seatNumber":"A4",
                "price":1190
            },
            {
                "seatNumber":"B4",
                "price":1190
            },
            {
                "seatNumber":"C4",
                "price":1190
            },
            {
                "seatNumber":"D4",
                "price":1190
            },
            {
                "seatNumber":"E4",
                "price":1190
            },
            {
                "seatNumber":"F4",
                "price":1190
            },
            {
                "seatNumber":"A2",
                "price":1590
            },
            {
                "seatNumber":"B2",
                "price":1590
            },
            {
                "seatNumber":"C2",
                "price":1590
            },
            {
                "seatNumber":"D2",
                "price":1590
            },
            {
                "seatNumber":"E2",
                "price":1590
            },
            {
                "seatNumber":"F2",
                "price":1590
            },
            {
                "seatNumber":"A5",
                "price":1990
            },
            {
                "seatNumber":"B5",
                "price":1990
            },
            {
                "seatNumber":"C5",
                "price":1990
            },
            {
                "seatNumber":"D5",
                "price":1990
            },
            {
                "seatNumber":"E5",
                "price":1990
            },
            {
                "seatNumber":"F5",
                "price":1990
            },
            {
                "seatNumber":"A3",
                "price":1990
            },
            {
                "seatNumber":"B3",
                "price":1990
            },
            {
                "seatNumber":"C3",
                "price":1990
            },
            {
                "seatNumber":"D3",
                "price":1990
            },
            {
                "seatNumber":"E3",
                "price":1990
            },
            {
                "seatNumber":"F3",
                "price":1990
            },
            {
                "seatNumber":"A1",
                "price":1090
            },
            {
                "seatNumber":"B1",
                "price":1090
            },
            {
                "seatNumber":"C1",
                "price":1090
            },
            {
                "seatNumber":"D1",
                "price":1090
            },
            {
                "seatNumber":"E1",
                "price":1090
            },
            {
                "seatNumber":"F1",
                "price":1090
            }
        ],
        "startTime":1724238000,
        "endTime":1725238000,
        "driverDetails":{
            "contactNumber":"8160554167",
            "name":"Prateek Sharma"
        }
    },


    {
        "busId":"51bc03gcd233f5c2b3a24b34",
        "source":"64fa03fcd233f6c2b3a25b34",
        "destination":"74fa03fcd233f7c2b3a84b34",
        "boardingPoints":[
            {
                "stopId":21,
                "arrivalTime":1724238000
            },
            {
                "stopId":22,
                "arrivalTime":1724241600
            },
            {
                "stopId":23,
                "arrivalTime":1724243400
            },
            {
                "stopId":24,
                "arrivalTime":1724245200
            },
            {
                "stopId":25,
                "arrivalTime":1724249700
            },
            {
                "stopId":26,
                "arrivalTime":1724255100
            },
            {
                "stopId":27,
                "arrivalTime":1724257200
            },
            {
                "stopId":28,
                "arrivalTime":1724261400
            }
        ],
        "droppingPoints":[
            {
                "stopId":24,
                "arrivalTime":1724245200
            },
            {
                "stopId":25,
                "arrivalTime":1724249700
            },
            {
                "stopId":26,
                "arrivalTime":1724255100
            },
            {
                "stopId":27,
                "arrivalTime":1724257200
            },
            {
                "stopId":28,
                "arrivalTime":1724261400
            },
            {
                "stopId":29,
                "arrivalTime":1724263200
            },
            {
                "stopId":30,
                "arrivalTime":1724264400
            },
            {
                "stopId":31,
                "arrivalTime":1724265900
            }
        ],
        "prices":[
            {
                "seatNumber":"A6",
                "price":1130
            },
            {
                "seatNumber":"B6",
                "price":1130
            },
            {
                "seatNumber":"C6",
                "price":1130
            },
            {
                "seatNumber":"D6",
                "price":1130
            },
            {
                "seatNumber":"E6",
                "price":1130
            },
            {
                "seatNumber":"F6",
                "price":1130
            },
            {
                "seatNumber":"A4",
                "price":1130
            },
            {
                "seatNumber":"B4",
                "price":1130
            },
            {
                "seatNumber":"C4",
                "price":1130
            },
            {
                "seatNumber":"D4",
                "price":1130
            },
            {
                "seatNumber":"E4",
                "price":1130
            },
            {
                "seatNumber":"F4",
                "price":1130
            },
            {
                "seatNumber":"A2",
                "price":1210
            },
            {
                "seatNumber":"B2",
                "price":1210
            },
            {
                "seatNumber":"C2",
                "price":1210
            },
            {
                "seatNumber":"D2",
                "price":1210
            },
            {
                "seatNumber":"E2",
                "price":1210
            },
            {
                "seatNumber":"F2",
                "price":1210
            },
            {
                "seatNumber":"A5",
                "price":1000
            },
            {
                "seatNumber":"B5",
                "price":1000
            },
            {
                "seatNumber":"C5",
                "price":1000
            },
            {
                "seatNumber":"D5",
                "price":1000
            },
            {
                "seatNumber":"E5",
                "price":1000
            },
            {
                "seatNumber":"F5",
                "price":1000
            },
            {
                "seatNumber":"A3",
                "price":1000
            },
            {
                "seatNumber":"B3",
                "price":1000
            },
            {
                "seatNumber":"C3",
                "price":1000
            },
            {
                "seatNumber":"D3",
                "price":1000
            },
            {
                "seatNumber":"E3",
                "price":1000
            },
            {
                "seatNumber":"F3",
                "price":1000
            },
            {
                "seatNumber":"A1",
                "price":1110
            },
            {
                "seatNumber":"B1",
                "price":1110
            },
            {
                "seatNumber":"C1",
                "price":1110
            },
            {
                "seatNumber":"D1",
                "price":1110
            },
            {
                "seatNumber":"E1",
                "price":1110
            },
            {
                "seatNumber":"F1",
                "price":1110
            }
        ],
        "startTime":1624238000,
        "endTime":1723238000,
        "driverDetails":{
            "contactNumber":"9812554167",
            "name":"Rajesh Rajput"
        }
    },



    {
        "busId":"63dc03hcd233f5c2b3a24b34",
        "source":"64fa04acd233f5c2b3a24b34",
        "destination":"74fd83fcd233f5c2b3a24b34",
        "boardingPoints":[
            {
                "stopId":41,
                "arrivalTime":1724238000
            },
            {
                "stopId":42,
                "arrivalTime":1724241600
            },
            {
                "stopId":43,
                "arrivalTime":1724243400
            },
            {
                "stopId":44,
                "arrivalTime":1724245200
            },
            {
                "stopId":45,
                "arrivalTime":1724249700
            },
            {
                "stopId":46,
                "arrivalTime":1724255100
            },
            {
                "stopId":47,
                "arrivalTime":1724257200
            },
            {
                "stopId":48,
                "arrivalTime":1724261400
            }
        ],
        "droppingPoints":[
            {
                "stopId":44,
                "arrivalTime":1724245200
            },
            {
                "stopId":45,
                "arrivalTime":1724249700
            },
            {
                "stopId":46,
                "arrivalTime":1724255100
            },
            {
                "stopId":47,
                "arrivalTime":1724257200
            },
            {
                "stopId":48,
                "arrivalTime":1724261400
            },
            {
                "stopId":49,
                "arrivalTime":1724263200
            },
            {
                "stopId":50,
                "arrivalTime":1724264400
            },
            {
                "stopId":51,
                "arrivalTime":1724265900
            }
        ],
        "prices":[
            {
                "seatNumber":"A6",
                "price":1690
            },
            {
                "seatNumber":"B6",
                "price":1690
            },
            {
                "seatNumber":"C6",
                "price":1690
            },
            {
                "seatNumber":"D6",
                "price":1690
            },
            {
                "seatNumber":"E6",
                "price":1690
            },
            {
                "seatNumber":"F6",
                "price":1690
            },
            {
                "seatNumber":"A4",
                "price":1690
            },
            {
                "seatNumber":"B4",
                "price":1690
            },
            {
                "seatNumber":"C4",
                "price":1690
            },
            {
                "seatNumber":"D4",
                "price":1690
            },
            {
                "seatNumber":"E4",
                "price":1690
            },
            {
                "seatNumber":"F4",
                "price":1690
            },
            {
                "seatNumber":"A2",
                "price":1890
            },
            {
                "seatNumber":"B2",
                "price":1890
            },
            {
                "seatNumber":"C2",
                "price":1890
            },
            {
                "seatNumber":"D2",
                "price":1890
            },
            {
                "seatNumber":"E2",
                "price":1890
            },
            {
                "seatNumber":"F2",
                "price":1890
            },
            {
                "seatNumber":"A5",
                "price":1790
            },
            {
                "seatNumber":"B5",
                "price":1790
            },
            {
                "seatNumber":"C5",
                "price":1790
            },
            {
                "seatNumber":"D5",
                "price":1790
            },
            {
                "seatNumber":"E5",
                "price":1790
            },
            {
                "seatNumber":"F5",
                "price":1790
            },
            {
                "seatNumber":"A3",
                "price":1790
            },
            {
                "seatNumber":"B3",
                "price":1790
            },
            {
                "seatNumber":"C3",
                "price":1790
            },
            {
                "seatNumber":"D3",
                "price":1790
            },
            {
                "seatNumber":"E3",
                "price":1790
            },
            {
                "seatNumber":"F3",
                "price":1790
            },
            {
                "seatNumber":"A1",
                "price":1890
            },
            {
                "seatNumber":"B1",
                "price":1890
            },
            {
                "seatNumber":"C1",
                "price":1890
            },
            {
                "seatNumber":"D1",
                "price":1890
            },
            {
                "seatNumber":"E1",
                "price":1890
            },
            {
                "seatNumber":"F1",
                "price":1890
            }
        ],
        "startTime":1524238000,
        "endTime":1624238000,
        "driverDetails":{
            "contactNumber":"6590554167",
            "name":"Vikalp Thakur"
        }
    },



    {
        "busId":"53ac03fcd233f5c2b3a24b34",
        "source":"64fa03fab233f5c2b3a24b34",
        "destination":"74fa02gcd233f5c2b3a24b34",
        "boardingPoints":[
            {
                "stopId":51,
                "arrivalTime":1724238000
            },
            {
                "stopId":52,
                "arrivalTime":1724241600
            },
            {
                "stopId":53,
                "arrivalTime":1724243400
            },
            {
                "stopId":54,
                "arrivalTime":1724245200
            },
            {
                "stopId":55,
                "arrivalTime":1724249700
            },
            {
                "stopId":56,
                "arrivalTime":1724255100
            },
            {
                "stopId":57,
                "arrivalTime":1724257200
            },
            {
                "stopId":58,
                "arrivalTime":1724261400
            }
        ],
        "droppingPoints":[
            {
                "stopId":54,
                "arrivalTime":1724245200
            },
            {
                "stopId":55,
                "arrivalTime":1724249700
            },
            {
                "stopId":56,
                "arrivalTime":1724255100
            },
            {
                "stopId":57,
                "arrivalTime":1724257200
            },
            {
                "stopId":58,
                "arrivalTime":1724261400
            },
            {
                "stopId":59,
                "arrivalTime":1724263200
            },
            {
                "stopId":60,
                "arrivalTime":1724264400
            },
            {
                "stopId":61,
                "arrivalTime":1724265900
            }
        ],
        "prices":[
            {
                "seatNumber":"A6",
                "price":1290
            },
            {
                "seatNumber":"B6",
                "price":1290
            },
            {
                "seatNumber":"C6",
                "price":1290
            },
            {
                "seatNumber":"D6",
                "price":1290
            },
            {
                "seatNumber":"E6",
                "price":1290
            },
            {
                "seatNumber":"F6",
                "price":1290
            },
            {
                "seatNumber":"A4",
                "price":1290
            },
            {
                "seatNumber":"B4",
                "price":1290
            },
            {
                "seatNumber":"C4",
                "price":1290
            },
            {
                "seatNumber":"D4",
                "price":1290
            },
            {
                "seatNumber":"E4",
                "price":1290
            },
            {
                "seatNumber":"F4",
                "price":1290
            },
            {
                "seatNumber":"A2",
                "price":1590
            },
            {
                "seatNumber":"B2",
                "price":1590
            },
            {
                "seatNumber":"C2",
                "price":1590
            },
            {
                "seatNumber":"D2",
                "price":1590
            },
            {
                "seatNumber":"E2",
                "price":1590
            },
            {
                "seatNumber":"F2",
                "price":1590
            },
            {
                "seatNumber":"A5",
                "price":1990
            },
            {
                "seatNumber":"B5",
                "price":1990
            },
            {
                "seatNumber":"C5",
                "price":1990
            },
            {
                "seatNumber":"D5",
                "price":1990
            },
            {
                "seatNumber":"E5",
                "price":1990
            },
            {
                "seatNumber":"F5",
                "price":1990
            },
            {
                "seatNumber":"A3",
                "price":1990
            },
            {
                "seatNumber":"B3",
                "price":1990
            },
            {
                "seatNumber":"C3",
                "price":1990
            },
            {
                "seatNumber":"D3",
                "price":1990
            },
            {
                "seatNumber":"E3",
                "price":1990
            },
            {
                "seatNumber":"F3",
                "price":1990
            },
            {
                "seatNumber":"A1",
                "price":1090
            },
            {
                "seatNumber":"B1",
                "price":1090
            },
            {
                "seatNumber":"C1",
                "price":1090
            },
            {
                "seatNumber":"D1",
                "price":1090
            },
            {
                "seatNumber":"E1",
                "price":1090
            },
            {
                "seatNumber":"F1",
                "price":1090
            }
        ],
        "startTime":1524538000,
        "endTime":1634235000,
        "driverDetails":{
            "contactNumber":"5715554167",
            "name":"Krishan Kant Chaudhary"
        }
    }
]
```

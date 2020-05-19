# VCIS Request URIs

## `/info`

>**Refer to [to the info JSON schema](vci_info_schema.json) for the official structure for this data.**

**Only the `GET` verb is supported.**

Every piece of vehicle information that can be provided is conceptually modeled as a JSON dictionary.  Each path component in the request URI is used to address the data item of interest.  For example, making a request to `/info` will yield everything that is available, for example:

```json
{
    "vehicle": {
        "brand": "Chrysler",
        "year": 2018,
        "make": "300",
        "vin": ""
    },
    "state": {
        "dynamics": {
            "speed": 25,
            "acceleration": {
                "lateral": -0.5,
                "longitudinal": 1
            }
        },
        "powertrain": {
            "torque": {
                "minimum": -0.10,
                "maximum": 212,
                "effective": 111
            },
            "rpm": 0,
            "starter-running": true,
            "throttle-position": 0.5
        },
        "transmission": {
            "gear-selection": "park""
        }
    }
}
```

To retreive only a piece of information, the request URI can be made more specific.  For example, a request for URI `/info/state/dynamics` would result in:

```json
{
    "speed": 25,
    "acceleration": {
        "lateral": -0.5,
        "longitudinal": 1
    }
}
```

The request URI can made further specific as to retrieve the scalar value.  For example, a request for URI `/info/state/dynamics/speed` would result in:

```json
25
```

### The `?supported` Query Parameter

When appended to any URI inside `/info`, the result is an array of relative
request URIs that the server supports providing a value for.

For example, requesting `/info/state/powertrain?supported` may result in:

```json
["torque", "rpm", "starter-running", "throttle-position"]
```

> *Note*: A request for a non-existant or unsupported data item will result in a `404 Not Found` error response.

-------------

## `/control`

>**Refer to [to the control JSON schema](vci_control_schema.json) for the official structure for this data.**

All controllable aspects (herein each referred to as an "actuator") of the vehicle are conceptually modeled as a JSON dictionary, using each of the request URI's path components to address the actuator to be controlled.

### **Querying Actuator Control State**

The requst verb `GET` may be used to retrieve the current state of the actuator.  Like performing a `GET /info` as described above, a `GET /control` would result in something like:

```json
{
    "motion": {
        "longitudinal": {
            "power": {},
            "brake": {
                "acceleration": 0.10812
            }
        },
        "lateral": {
            "magnitude": -52.8512
        }
    },
    "driver-actuation": {
        "turn-signal": {},
        "horn": {}
    },
    "lighting": {
        "head": {
            "left": "on",
            "right": "on"
        },
        "fog": {
            "left": {},
            "right": {}
        },
        "reverse": {
            "left": {},
            "right": {}
        },
        "parking": {
            "left": {},
            "right": {}
        },
        "tail": {
            "left": {},
            "right": {}
        },
        "brake": {
            "left": {},
            "right": {}
        },
        "daytime": {
            "left": {},
            "right": {}
        },
        "signal": {
            "left": {},
            "right": {}
        },
        "chmsl": {},
        "license-plate": {}
    }
}
```

### **Determining an Actuator's Supported Control Values**

The control values supported by the actuator referenced at the given request URI can be retrieved by appending the `?definition` query parameter.

For example, a vehicle that supports longitudinal control with power represented as torque and braking represented as an acceleration, and lateral control represented as a magnitude or torque, a request to `/control/motion?definition` will result in:

```json
{
    "motion": {
        "lateral": {
            "magnitude": {},
            "torque": {}
        },
        "longitudinal": {
            "power": {
                "magnitude": {},
                "torque": {}
            },
            "brake": {
                "magnitude": {},
                "acceleration": {
                    "minimum": 0.0,
                    "maximum": 9.0
                }
            }
        }
    }
}
```

A request to `/control/lighting/head?definition` will result in:

```json
{
    "left": [ "on", "off" ],
    "right": [ "on", "off" ]
}
```

### **Assigning an Actuator's Control Value**

The `PUT` request verb is used to assign a control value to the actuator at the given URI.  For example, to apply a counterclockwise force at half magnitude to the steering wheel, the request would look like:

`PUT /control/motion/lateral`

```json
{
    "magnitude": -50.0
}
```

Or to set lateral and longitudinal in the same request:

`PUT /control/motion`

```json
{
    "lateral": {
        "magnitude": -50.0
    },
    "longitudinal": {
        "power": {
            "torque": 202.251
        }
    }
}
```

### **Releasing Control of an Actuator**

Use the `DELETE` verb to release control of the actuator at the given URI.

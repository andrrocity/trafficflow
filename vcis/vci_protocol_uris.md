# VCIS Request URIs

## `/info`

>**Refer to [to the info JSON schema](vci_info_schema.json) for the official structure for this data.**

 The entire vehicle information tree is a JSON dictionary.  Each path component after `/info/` represents the key in the dictionary to drill in to.  For example, making a request to `/info` will yield every single bit of information available, for example:

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
            "gear-selection": "park/reverse/neutral/drive/unknown"
        }
    }
}
```

Now, if a request to `/info/state/dynamics` was made, the result would be:

```json
{
    "speed": 25,
    "acceleration": {
        "lateral": -0.5,
        "longitudinal": 1
    }
}
```

This can be drilled even further to get a single value,  `/info/state/dynamics/speed` the result would be:

```json
25
```

### The `?supported` Query Parameter

When appended to a request URI, the result will be an array of strings containing each supported value.

For example, requesting `/info/state/powertrain?supported` would result in:

```json
["torque/minimum", "torque/maximum", "torque/effective", "rpm", "starter-running", "throttle-position"]
```

-------------

## `GET /control`

>**Refer to [to the control JSON schema](vci_control_schema.json) for the official structure for this data.**

Peforming a `GET` to this request URI will return a JSON description of the supported lateral
and longitudial control methods.

For example, a vehicle that supports power represented as torque, but only supports braking value represented as an acceleration, the result may look like this:

```json
{
    "longitudinal": {
        "power": {
            "magnitude": {},
            "torque": {}
        },
        "braking": {
            "magnitude": {},
            "acceleration": {
                "minimum": 0.0,
                "maximum": 9.0
            }
        }
    }
}
```

-------------

## `PUT /control`

Sending a `PUT` request to the given URI will attempt to take control of the vehicle using the provided longidudinal/lateral values provided.

> **Note: When requesting longitudinal control, `braking` or `power` must both not be present at the same time.**

```json
{
    "longitudinal": {
        "power": {
            "magnitude": 50.0,
        },
        "braking": {
            "acceleration": 2.0,
        }
    },
    "lateral": {
        "magnitude": -2.0
    }
}
```

-------------

## `DELETE /control(/[longitudinal|lateral])`

Sending a `DELETE` request to `/control` will relinquish all control and return the vehicle to its original state.  To relinquish longitudinal or lateral control separately, make the request to `/control/longitudinal` or `/control/lateral` individually.

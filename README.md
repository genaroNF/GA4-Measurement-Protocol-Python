# GA4 Measurement Protocol Support for Python

This library provides an interface for sending data to Google Analytics, supporting the GA4 Measurement Protocol.

**NOTE**: This project is in *beta* and will be continually updated to cover relevant features of the GA4 Measurement Protocol. Please feel free to file issues for feature requests.

[Meet the next generation of Google Analytics: Learn about the new Google Analytics and how to get started](https://support.google.com/analytics/answer/10089681)

## Contact

Email: `analytics-help@adswerve.com`

## Installation

The easiest way to install GA4 Measurement Protocol Support for Python is directly from PyPi using `pip` by running the following command:

`pip install ga4mp`


## Usage
> **NOTE**: Recent changes have added new platform specific subclasses. In order to take advantage of new functionality, you will need to update the class name of the GA4 object(s) being created in your code.

This library supports both gtag and Firebase data collection models. When creating your tracking object, use either `gtagMP` or `firebaseMP`, depending on your needs.

The required credentials for sending events to GA4 using **gtag** comprise the following:

| Credential     | Description                                                                                                                                                                                               |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| api_secret     | Generated throught the Google Analytics UI. To create a new secret, navigate in the Google Analytics UI to: **Admin** > **Data Streams** > **choose your stream** > **Measurement Protocol API secrets** > **Create** |
| measurement_id | The identifier for a Data Stream. Found in the Google Analytics UI under:  **Admin** > **Data Streams** > **choose your stream** > **Measurement ID**                                                     |
| client_id      | A unique identifier for a client, representing a specific browser/device.                                                                                                                                 |

The required credentials for sending events to **Firebase** comprise the following:

| Credential      | Description                                                                                                                                                                                               |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| api_secret      | Generated throught the Google Analytics UI. To create a new secret, navigate in the Google Analytics UI to: **Admin** > **Data Streams** > **choose your stream** > **Measurement Protocol API secrets** > **Create** |
| firebase_app_id | The identifier for a Firebase app. Found in the Firebase console under: **Project Settings** > **General** > **Your Apps** > **App ID**.                                                                  |
| app_instance_id | A unique identifier for a Firebase app instance. See [Required parameters > 2. JSON body](https://developers.google.com/analytics/devguides/collection/protocol/ga4/sending-events?client_type=firebase#required_parameters) for details. |

Create your *credentials.json* file and put in your "./credentials" subdirectory.

``` json
{"API_SECRET": "<YOUR_API_SECRET>",
 "MEASUREMENT_ID": "<YOUR_MEASUREMENT_ID>",
 "CLIENT_ID": "<YOUR_CLIENT_ID>",
 "FIREBASE_APP_ID": "<YOUR_FIREBASE_APP_ID>",
 "APP_INSTANCE_ID": "<YOUR_APP_INSTANCE_ID>"}
```
The following represents a simple example of a custom event sent to GA4:
``` python
from ga4mp import gtagMP, firebaseMP

# Create an instance of GA4 object using gtag...
ga = gtagMP(api_secret = <API_SECRET>, measurement_id = <MEASUREMENT_ID>, client_id=<CLIENT_ID>)

# ...or create an object using Firebase.
ga = firebaseMP(api_secret = <API_SECRET>, firebase_app_id=<FIREBASE_APP_ID>, app_instance_id=<CLIENT_ID>)

# Specify event type and parameters
event_type = 'new_custom_event'
event_parameters = {'parameter_key_1': 'parameter_1', 'parameter_key_2': 'parameter_2'}
event = {'name': event_type, 'params': event_parameters }
events = [event]

"""
Events need to be passed as a list of dictionaries, fitting the format:
[{'name': 'level_end',
  'params' : {'level_name': 'First',
              'success': 'True'}
 },
 {'name': 'level_up',
  'params': {'character': 'John Madden',
             'level': 'First'}
 }]
"""

# Set persistent user properties
# Includes user_id, non_personalized_ads, and all else set as custom user_properties
ga.set_user_property('user_id', 'Thales2000')
ga.set_user_property('customer_tier','enterprise')

# Remove a user property
ga.delete_user_property('user_id')

# Send a custom event to GA4 immediately
ga.send(events)

# Postponed send of a custom event to GA4
ga.send(events, postpone=True)
ga.postponed_send()

# Generate and set a new, random Client ID (gtagMP objects only)
ga.client_id = ga.random_client_id()
```

## How to construct Events
For more information on constructing events, please review the [GA4 Measurement Protocol reference](https://developers.google.com/analytics/devguides/collection/protocol/ga4/reference).

## User properties
For more information on what user properties are in GA4 and what you can do with them, [please review here](https://developers.google.com/analytics/devguides/collection/protocol/ga4/user-properties?client_type=gtag)

## License

GA4 Measurement Protocol Support for Python is licensed under the [BSD License](./LICENSE).

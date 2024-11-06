# Event Manager API Documentation
[Home](README.md) | [Up](../README.md)

The Event Manager API by ExampleSoft allows developers to create, manage, and track events. This API provides endpoints for event creation, attendee management, and RSVP handling, making it ideal for applications that require an event management feature. 

## Table of Contents
- [Event Manager API Documentation](#event-manager-api-documentation)
  - [Table of Contents](#table-of-contents)
  - [Authorization](#authorization)
  - [Quick Start](#quick-start)
  - [API Endpoints](#api-endpoints)
    - [List Events](#list-events)
    - [Get Event Details](#get-event-details)
    - [Get Attendees](#get-attendees)
    - [Create Event](#create-event)
    - [Update Event](#update-event)
    - [Delete Event](#delete-event)
    - [RSVP](#rsvp)
  - [Error Codes and Handling](#error-codes-and-handling)
  - [Sample Client Program](#sample-client-program)

## Authorization

To access the Event Manager API, a valid Bearer token is required. Treat this token as a secret and store it securely, using a key management system if possible. Contact your account manager if you need help generating your token.

**Authorization Header Example**:
```http
Authorization: Bearer <YOUR_TOKEN_HERE>
```

Replace `<YOUR_TOKEN_HERE>` with your actual token. Include this header in each request to authenticate with the API.

---

## Quick Start

1. Obtain an API token by contacting your account manager.
2. Include your token in the `Authorization` header for every request.
3. Start by listing all events to familiarize yourself with the API structure.

---

## API Endpoints

### List Events
- **Description**: Retrieves a list of all upcoming events. You can optionally filter by date.
- **Endpoint**: `GET /events`
- **Query Parameters**:
  - `date` (string, optional) - Filter events for a specific date in `YYYY-MM-DD` format.
- **Request Example**:
  ```http
  GET /events?date=2024-11-10
  Authorization: Bearer <YOUR_TOKEN_HERE>
  ```
- **Response Example**:
  ```json
  {
    "events": [
      {
        "id": 101,
        "name": "Tech Innovators Conference",
        "location": "San Francisco, CA",
        "date": "2024-11-10",
        "time": "09:00 AM",
        "capacity": 300,
        "attendees_count": 150
      },
      {
        "id": 102,
        "name": "AI & Future Forum",
        "location": "New York, NY",
        "date": "2024-11-12",
        "time": "10:00 AM",
        "capacity": 500,
        "attendees_count": 320
      }
    ]
  }
  ```

### Get Event Details
- **Description**: Retrieves detailed information about a specific event.
- **Endpoint**: `GET /events/{event_id}`
- **Path Parameter**:
  - `event_id` (integer, required) - The ID of the event to retrieve.
- **Request Example**:
  ```http
  GET /events/101
  Authorization: Bearer <YOUR_TOKEN_HERE>
  ```
- **Response Example**:
  ```json
  {
    "id": 101,
    "name": "Tech Innovators Conference",
    "location": "San Francisco, CA",
    "date": "2024-11-10",
    "time": "09:00 AM",
    "capacity": 300,
    "description": "A conference showcasing the latest tech innovations.",
    "attendees_count": 150,
    "attendees": [
      {"id": 1, "name": "Alice Johnson"},
      {"id": 2, "name": "Bob Smith"}
    ]
  }
  ```

### Get Attendees
- **Description**: Retrieves the list of attendees for a specific event.
- **Endpoint**: `GET /events/{event_id}/attendees`
- **Path Parameter**:
  - `event_id` (integer, required) - The ID of the event.
- **Request Example**:
  ```http
  GET /events/101/attendees
  Authorization: Bearer <YOUR_TOKEN_HERE>
  ```
- **Response Example**:
  ```json
  {
    "attendees": [
      {"id": 1, "name": "Alice Johnson"},
      {"id": 2, "name": "Bob Smith"}
    ]
  }
  ```

### Create Event
- **Description**: Creates a new event.
- **Endpoint**: `POST /events`
- **Request Body**:
  - `name` (string, required) - Name of the event.
  - `location` (string, required) - Location of the event.
  - `date` (string, required) - Date in `YYYY-MM-DD` format.
  - `time` (string, required) - Time of the event.
  - `capacity` (integer, required) - Maximum attendees allowed.
  - `description` (string, optional) - Brief description of the event.
- **Request Example**:
  ```http
  POST /events
  Authorization: Bearer <YOUR_TOKEN_HERE>
  Content-Type: application/json

  {
    "name": "Python Workshop",
    "location": "Online",
    "date": "2024-11-20",
    "time": "10:00 AM",
    "capacity": 100,
    "description": "An introductory workshop on Python programming."
  }
  ```
- **Response Example**:
  ```json
  {
    "message": "Event created successfully",
    "event_id": 103
  }
  ```

### Update Event
- **Description**: Updates an existing event with new information.
- **Endpoint**: `PUT /events/{event_id}`
- **Path Parameter**:
  - `event_id` (integer, required) - The ID of the event to update.
- **Request Body**: Only the fields to be updated are required.
  - `name`, `location`, `date`, `time`, `capacity`, `description` (optional).
- **Request Example**:
  ```http
  PUT /events/103
  Authorization: Bearer <YOUR_TOKEN_HERE>
  Content-Type: application/json

  {
    "location": "Los Angeles, CA",
    "time": "02:00 PM"
  }
  ```
- **Response Example**:
  ```json
  {
    "message": "Event updated successfully"
  }
  ```

### Delete Event
- **Description**: Deletes a specific event.
- **Endpoint**: `DELETE /events/{event_id}`
- **Path Parameter**:
  - `event_id` (integer, required) - The ID of the event to delete.
- **Request Example**:
  ```http
  DELETE /events/103
  Authorization: Bearer <YOUR_TOKEN_HERE>
  ```
- **Response Example**:
  ```json
  {
    "message": "Event deleted successfully"
  }
  ```

### RSVP
- **Description**: RSVP an attendee for a specific event.
- **Endpoint**: `POST /events/{event_id}/rsvp`
- **Path Parameter**:
  - `event_id` (integer, required) - The ID of the event.
- **Request Body**:
  - `attendee_name` (string, required) - Name of the attendee.
- **Request Example**:
  ```http
  POST /events/101/rsvp
  Authorization: Bearer <YOUR_TOKEN_HERE>
  Content-Type: application/json

  {
    "attendee_name": "Jane Doe"
  }
  ```
- **Response Example**:
  ```json
  {
    "message": "RSVP successful",
    "attendee_id": 3
  }
  ```

---

## Error Codes and Handling

| Code | Message           | Description                                             |
|------|--------------------|---------------------------------------------------------|
| 400  | Bad Request        | Invalid input or missing required parameters.           |
| 401  | Unauthorized       | Authentication token is missing or invalid.             |
| 404  | Not Found          | Resource not found; check IDs and endpoint paths.       |
| 500  | Internal Server Error | Server error; try again later or contact support.   |

If an error occurs, verify parameters, ensure a valid token, and check network connections.

---

## Sample Client Program

Here’s a Python example using the `requests` library to interact with the API. Replace `<YOUR_TOKEN_HERE>` with your actual token.

```python
import requests

BASE_URL = "https://api.eventmanager.com/v1"
headers = {
    "Authorization": "Bearer <YOUR_TOKEN_HERE>",
    "Content-Type": "application/json"
}

response = requests.get(f"{BASE_URL}/events", headers=headers)
print(response.json())
```
------

[Home](README.md) | [Up](../README.md)

# NoticeBoardTracker — James Barnett Submission

Backend for the NoticeBoardTracker project: a single AWS Lambda function (`backend/lambda_function.py`) behind an API Gateway HTTP API, storing data in MongoDB (`noticeboard_db`).

## Pain point addressed

The project brief lists "untracked/missing trainees and duplicated entries" as a problem with the current Excel-based tracking. `POST /trainees` gives trainees a single place to be registered and rejects duplicates by email.

## Endpoints

| Method | Path            | Description                              |
| ------ | --------------- | ---------------------------------------- |
| GET    | `/notices`      | List all notices                         |
| GET    | `/notices/{id}` | Get one notice                           |
| POST   | `/notices`      | Create a notice (`title`, `content`)     |
| PUT    | `/notices/{id}` | Update a notice                          |
| DELETE | `/notices/{id}` | Delete a notice                          |
| POST   | `/trainees`     | Register a trainee, rejecting duplicates |

### POST /trainees

Request body:

```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "cohort": "2026-fall"
}
```

Responses:

- `201` — trainee created; returns the stored trainee with its `_id`
- `400` — `email` is missing
- `409` — a trainee with that email already exists

Emails are trimmed and lowercased before the duplicate check, so `Jane@Example.com` and `jane@example.com` are treated as the same trainee.

## Configuration

- `MONGO_URI` — MongoDB connection string (Lambda environment variable)
- Dependencies: `pymongo`

## Deployment note

This submission is code only. To deploy, each path above needs a matching route in API Gateway pointing at the Lambda, including `POST /trainees`.
